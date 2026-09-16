# 021 · QKV finish 融合用反向顺序复测确认小收益

结果：**保留**。历史代码位置：`4de57c9`。

同一套复测规则也允许确认小收益；固定测量顺序后再看结果。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

已优化 QKV GEMM 后仍有独立 factor/bias/RoPE/scatter，产生一个 finish launch 和 projected 中间缓冲区。

## 当时看到了哪些证据

源码确认 RoPE 的相邻列配对落在现有 32 列 tile 内，Q/K/V 分界也对齐。调用者已传入 K/V suffix view，重复加 prefix offset 会写错地址。原 finish 的计算迁入 epilogue 后仍要执行，不能把原时间全算成收益。

## 提出了什么假设

在现有 tile 中显式保留 accumulator 的 BF16 roundtrip，再完成后处理和分散写出，可能删除 256,000-byte scratch 与一次启动，资源代价需要编译确认。

## 执行了哪些命令、做了什么改动

运行 `lab.pi05.expert_qkv_finish_screen`，检查 18 层 Q/K/V、factor 及 prefix 内容，核对 PTX 和资源；接入唯一候选后做官方 parity 和部署 ABBA。首轮 B 漂移接近收益，于是预先限定一次 BAAB 复测，全部八个结果共同判断。

## 得到了什么结果

局部输出及 prefix 逐位一致；仍为 40 registers、6 KiB shared、无 spills。局部链 A10.807111/10.775111 → B9.637333/9.630222 µs。首轮部署平均改善 0.122573 ms，但 B 漂移 0.100344 ms；反向块改善 0.098972 ms，A/B 漂移 0.002277/0.023303 ms，最小分离 0.086182 ms。

## 为什么保留、回退或继续调查

两种顺序均保持候选/对照中位数分离，保留。全部八次平均中位数差为 0.110772 ms。曲线使用的首个候选 32.041923 ms 只是一个记录点，不能独自充当该改动的增益估计。与案例 018 配对阅读，可看到同样追加协议得到不同决策。

## 如何核对这段过程

本摘录有 110 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"agent_message": 8, "function_call": 9, "function_call_output": 9}`；这些数量不是独立任务数。我们不补写缺失原话。

- [lab/pi05/expert_qkv_finish_screen.md](evidence/lab/pi05/expert_qkv_finish_screen.md)
- [当时保存的实验总结](evidence/run-note.md)

<details>
<summary>按时间查看当时公开的进展说明（原文，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T21:11:52.499Z | backbone | L1912 | 我会先确认 RoPE 的相邻列配对和 Q/K/V 地址布局，再评估能否直接在现有 tile 的累加器上完成后处理。重点是保留 BF16 舍入，并核算增加的寄存器与访存是否会抵消一次 finish launch 的节省。 |
| 2026-09-14T21:13:59.402Z | backbone | L1945 | 相邻的 RoPE 列都落在现有 32 列 tile 内，因此优先方案只需一个 dot 累加器，舍入后再拆成偶/奇列。Q、K、V 的边界也都与 tile 对齐，可直接写入各自布局。主要不确定性是 epilogue 增加的寄存器和布局转换成本。 |
| 2026-09-14T21:14:27.974Z | backbone | L1949 | 值得做一个固定 tile 的短 probe。**优先保留现有单个 dot 累加器，不需要两组 dot。**  现有 `16×32×32` tile 包含完整相邻列对，可按以下顺序融合：  1. FP32 accumulator → **BF16 RN → FP32**。 2. 分别执行 FP32 `projected * factor`，然后 `+ bias`。 3. reshape 为 `(16,16,2)`，用 `tl.split` 得到偶/奇列。 4. Q/K 执行原公式 `a0*cos-a1*sin`、`a1*cos+a0*sin`；V 直接转换 BF16。 5. 写 Q/K/V，保留 `enable_fp_fusion=False`、`enable_reflect_ftz=False`。  实际布局由 `fused_qkv.cu:40` 和 `h100/pi05/pipeline.py:203` 确认：  / 投影列 / 输出地址 / /---/---/ / 0–2047 / `Q + row*2048 + col` / / 2048–2303 / `K + row*256 + col-2048` / / 2304–2559 / `V + row*256 + col-2304` /  K/V 参数已经是 `kv[layer][prefix_len:]` 切片指针，**不能再次增加 prefix offset**。三个边界均按 32 列对齐；仍为 320 CTA，其中 Q 256、K/V 各 32。RoPE 按 `pair % 128` 取该行相邻两个系数。  算法层面不会增加权重读取或 GEMM FLOPs。两组 dot 虽然可计算偶/奇列，却引入 stride-2 权重加载及潜在重复事务，没有必要先承担这项复杂度。  主要风险是新增 epilogue 的寄存器、shuffle/layout conversion 和尾部耗时。当前赢家为 40 registers、6 KiB shared memory；原 PTX 已有输出布局转换，不能假定融合后的转换免费。需要检查实际资源、BF16 roundtrip 和运算指令顺序。  可用空间：  - 010 finish：**1.210 µs/次，0.218 ms/模型**，但其算术会移入 GEMM。 - 去掉 projected 写读：**512,000 B/次，87.89 MiB/模型**。 - 写读时间不能再直接加到 finish 时间上，且 scratch 可能命中 L2。粗略看，新增 epilogue 若让 GEMM 慢约 1.2 µs，就可能抵消 launch 节省。  最小实验只保留 `16×32×32`，对照现有 GEMM＋native finish；真实 18 层检查 Q/K/V、factor 和 prefix 区域，再做一次完整链 ABBA。失败或无收益便停止，不扩配置。  本次仅 CPU 源码、PTX分析；未编写生产代码或运行 GPU/JIT。 |
| 2026-09-14T21:15:29.149Z | backbone | L1960 | 我会准备一个固定 tile 的融合实验，复用现有真实调用记录方式，并加上 K/V prefix 区域检查。只比较同条件下的完整 QKV 链，不扩配置；现在仍只写 CPU 侧代码。 |
| 2026-09-14T21:19:25.078Z | backbone | L2005 | 融合脚本已写好，仍使用 320 个 CTA、4 warps、3 stages。正确性检查会保存真实 prefix 内容，并将输出写入带相同前缀长度的 K/V 缓冲区切片，确认 Q/K/V 数值正确且 prefix 未被覆盖；复制均在计时外完成。 |
| 2026-09-14T21:20:58.953Z | backbone | L2028 | 已提交 **`2b4ee7f`**，仅新增 lab probe 和说明。  固定 **16×32×32、320 CTA、4 warps、3 stages**。真实 18 层数值与 prefix 检查通过后，执行一次完整链 ABBA；失败或无收益即停止。  AST、依赖和差异检查通过。未运行 GPU/JIT，等待 QK 筛选后的窗口。 |
| 2026-09-14T21:24:12.265Z | backbone | L2041 | 开始运行单 tile QKV finish 融合 probe。先检查 18 层 Q/K/V、factor 和 prefix，再做一次完整链 ABBA；成功或失败后都会立即释放 GPU。 |
| 2026-09-14T21:24:58.563Z | backbone | L2061 | 融合候选通过了全部 18 层数值和 prefix 检查，GPU 已释放。完整链 ABBA 中，控制为 10.807/10.775 µs，候选为 9.637/9.630 µs；此次漂移小于测得收益。接下来只整理 PTX 和资源用量证据。 |
| 2026-09-14T21:26:44.104Z | backbone | L2085 | PTX 确认了 BF16 roundtrip、factor、bias 和 RoPE 的运算顺序；融合后仍是 40 个寄存器、6 KiB shared memory，且没有 spill。现在将这一个候选加入独立 backend，复用原 prepare，并去掉 projected scratch；只做 CPU 声明检查。 |
| 2026-09-14T21:36:28.426Z | main | L3984 | QK 这轮已保留：四轮对照的平均中位数改善约 0.193 ms，候选为 32.172 / 32.211 ms。下一轮只接入 QKV 末尾融合，验证减少中间读写和一次启动是否能进一步降低端到端延迟。  |
| 2026-09-14T21:42:09.952Z | main | L4027 | QKV 末尾融合已通过官方对齐检查和目标路由检查。接下来完成同条件的 ABBA 端到端对照；backbone 的 up+GELU 融合已准备好源码，等这一轮计时结束后再编译，避免干扰测量。  |
| 2026-09-14T21:43:35.115Z | main | L4076 | QKV 融合的两次候选测量已经完成，正在跑最后一次旧方案对照。官方对齐的 action 误差与上一版相同；性能结论仍要看四轮差值是否明显超过漂移。  |
| 2026-09-14T21:44:42.824Z | main | L4107 | QKV 融合四轮平均改善约 0.123 ms，但候选两次中位数相差 0.100 ms，已接近收益尺度。我会按前面小收益实验的做法，只补一组预先固定的反向 BAAB 对照，确认能否稳定分离；保留全部结果。  |
| 2026-09-14T21:46:22.828Z | main | L4155 | 这轮额外对照只改变运行顺序，输入、权重、精度和测量次数都保持一致。下一候选将把 backbone up 的 GELU/product 移入 GEMM 末尾；它可能省掉中间读写，但当前 GEMM 已用 254 个寄存器，编译后是否溢出会先决定它值不值得继续。  |
| 2026-09-14T21:47:27.590Z | main | L4184 | 反向对照确认了 QKV 融合的收益：平均改善约 0.099 ms，两路漂移约 0.002 / 0.023 ms，候选中位数仍全部低于旧方案。这轮保留；四次候选测得约 32.04–32.16 ms。现在开始 backbone up+GELU 的局部实验。  |
| 2026-09-14T21:50:26.548Z | main | L4239 | 021 的全部八次测量、决策记录和延迟曲线已提交，并同步到本地查看文件。另一个候选是 vision FFN 的 GELU 融合，旧 trace 中这部分总计约 0.09 ms；目前只准备源码，GPU 仍由 backbone 实验独占。  |

</details>
