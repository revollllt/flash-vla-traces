# 023 · 从真实 mask 发现 padding 空间，逐步验证动态 FFN 分桶

结果：**有条件保留**。历史代码位置：`7869a5a`。

先测廉价静态上限，再支付动态实现成本；正确性输入必须覆盖被优化的分支。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

backbone 缓冲区物理上有 968 行，但实际任务的有效语言前缀会变化，可能存在无需执行的尾部 GEMM 计算。

## 当时看到了哪些证据

两个实际 seed42 记录均为 127 个语言 token、895 个有效 prefix 行；旧 seed0 官方 fixture 是 903 行。静态假设不能硬编码 seed42 的长度。源码还显示原 GEMM 参数在 host 初始化，运行时选择、workspace 与旧尾部值都需要处理。

## 提出了什么假设

先用既有 cfg0 比较 M896 和 M968，判断收益是否足以支付动态实现；若有空间，再用设备 mask 在两个静态计划间选择。该选择依赖项目当前连续有效前缀布局，不适用于任意稀疏 mask。

## 执行了哪些命令、做了什么改动

先运行 `lab.pi05.runtime_prefix_static`，再设计 Target 内显式 mask 算子、两个独立 workspace 和同一图中的双入口。短路径在读取旧 gate/up 尾部前写零，外部 KV 物理布局与 suffix offset 保持 968。用新短 fixture 和旧长 fixture 运行 `lab.pi05.bucket_switch`，在同一 engine/graph 上验证 short896→long903→short896；最后连续运行四个独立进程 ABBA。

## 得到了什么结果

静态 51 GEMM 平均改善 1.673216 ms，控制漂移 0.290816 ms，只构成继续实现的理由。动态候选三次官方比较通过；部署 A32.061675/B30.315662/B30.315381/A32.147649 ms，平均改善 1.789140 ms，明显超过 A/B 漂移 0.085974/0.000281 ms。workspace 从约 22.3 MB 增至 44.6 MB。

## 为什么保留、回退或继续调查

保留，但性能结论只覆盖当前 valid_prefix≤896 的测量输入；长前缀只验证了数值，没有建立速度收益。原官方 fixture 不走短分支，所以必须增加边界输入。oracle 使用带本地修改的 vendored OpenPI 与兼容依赖，不能宣称纯净上游验证或机器人任务成功率提升。

## 如何核对这段过程

本摘录有 329 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"agent_message": 71, "function_call": 62, "function_call_output": 61}`；这些数量不是独立任务数。我们不补写缺失原话。

- [lab/pi05/padding_runtime_rows.md](evidence/lab/pi05/padding_runtime_rows.md)
- [lab/pi05/runtime_prefix_static.md](evidence/lab/pi05/runtime_prefix_static.md)
- [lab/pi05/runtime_prefix_native_feasibility.md](evidence/lab/pi05/runtime_prefix_native_feasibility.md)
- [lab/pi05/bucket_switch.md](evidence/lab/pi05/bucket_switch.md)
- [当时保存的实验总结](evidence/run-note.md)

<details>
<summary>按时间查看当时公开的进展说明（历史路径以代码显示，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T22:43:09.761Z | backbone | L2940 | 我会沿着输入准备、mask/position 构造和 backbone 到 expert 的 KV 消费路径检查，确认哪些 padding 行在数学上可省，以及当前算子接口是否提供了合法跳过它们的信息。  |
| 2026-09-14T22:43:59.337Z | main | L5194 | 已保存两项停止实验的原始结果。接下来先做 CPU 筛选：检查 padding token 是否存在可安全省去的计算、dual FFN 是否有未试过的布局机会，以及 softmax/PV 能否在保留 BF16 舍入的位置下融合；筛选结果决定下一次 GPU 实验。  |
| 2026-09-14T22:44:54.445Z | backbone | L2979 | 源码确认，padding 是每次随 state／task 变化的语言尾部，不能固定成 seed 42 的长度。官方检查只比较有效 prefix KV，padding KV 只要求有限值；因此数学上有跳过空间，但当前 QKV／FFN 接口没有收到有效长度或 mask，我正在核对这个接口边界。  |
| 2026-09-14T22:47:32.812Z | backbone | L3011 | 结论：**语义上允许跳过 padding 行，但当前接口不足以直接缩小 QKV／FFN GEMM。**  - 每次 forward，state 和 task 决定 `n_tokens`；host 更新 token IDs、embedding scale、mask、expert RoPE。有效长度是 `768+n_tokens`，不能固定使用 seed 42 的长度。prefix.py（历史路径 `/home/ubuntu/flash-vla/src/flash_vla/hardware/nvidia/h100/pi05/prefix.py:116`） - buffer 形状、backbone RoPE、suffix 物理偏移 **968** 固定。expert 逻辑位置则从 `n_valid` 开始。单次去噪过程中 prefix KV/mask 固定，suffix KV 每层每步更新。 - 当前 backbone attention 仍计算全部查询，仅屏蔽 padding **key**，没有避免 padding query 计算。attention.py（历史路径 `/home/ubuntu/flash-vla/src/flash_vla/hardware/nvidia/h100/pi05/backends/tilelang/kernels/attention.py:52`） - padding KV 后续被 dense attention 读取，但不贡献有效输出；现有 oracle 只要求它们有限，避免 `0×NaN` 污染。parity.py（历史路径 `/home/ubuntu/flash-vla/eval/pi05/parity.py:217`）  当前只有 attention 接收 mask；QKV、out-projection 和 FFN 均没有有效长度参数，已捕获的 `_Plan` 也不会动态改变 M。不能靠零 activation 判断 padding。  因此，现接口内可考虑 attention 的 runtime query-tile 跳过；要减少主要 GEMM 工作，需先解决 **Target 内部如何显式传递 runtime 有效长度**。本轮没有实现或 GPU 工作。 |
| 2026-09-14T22:48:09.813Z | backbone | L3024 | 我会进一步核对 Target 自定义算子参数的边界，以及现有 CUTLASS 内核是否能读取设备端有效长度。然后把 padding 语义和最小内部方案合并成一份可审阅的分析提交；本轮仍不改实现、不用 GPU。  |
| 2026-09-14T22:48:14.716Z | main | L5296 | Padding 分析发现了更大的潜在空间：语言有效长度会随 state/task 改变，但 backbone 仍计算全部 968 行。目前 QKV/FFN 没有长度参数，我正在确认能否显式传入，同时保留现有输入、KV 布局和图复用。GPU 正在验证一个较小的候选：dual FFN 只将 warp 数从 4 改为 8。  |
| 2026-09-14T22:50:14.709Z | main | L5331 | Dual FFN 的 8-warp 版本数值逐元素一致，但局部每次调用约慢 0.18 微秒，已停止。现在验证 expert down 的一个独立假设：保留原 kernel 和 epilogue，只改变 Split-K 调度参数，观察更多并行计算块与不同归约方式能否降低延迟。  |
| 2026-09-14T22:52:10.880Z | main | L5378 | Split-K 调度实验的 180 次真实调用均通过数值检查，但局部每次约慢 4.56 微秒，已停止。它没有改变 kernel 本体，因此这次否定的是该调度选择。下一项将验证 softmax/PV 融合；主要风险是重复 softmax 计算可能抵消省下的访存和启动开销。  |
| 2026-09-14T22:53:56.051Z | backbone | L3086 | 接口上已有一条显式路径：Target 可以新增算子名并把现有 mask 作为图参数传入，无需改共享 runner；标准算子名则不能直接追加参数。另一个关键发现是，已部署的 Stream-K cfg0 将尺寸固化在 host 参数中；vendor 的 `GemmGrouped(kDeviceOnly)` 才支持设备端 problem size，但它是不同的调度实现，性能需要重新验证。  |
| 2026-09-14T22:56:31.712Z | main | L5467 | 已记录这些负结果，部署仍是已通过验证的第 021 轮。Padding 路线正在收敛为一个具体方案：根据运行时 mask，在 896 行和 968 行两种内部计算范围间选择；这仍是待验证方案，关键是确保有效长度增长时新行会重算，且 padding 不污染 KV。  |
| 2026-09-14T22:57:41.294Z | backbone | L3115 | 我先核对延迟测试 seed 42 的实际语言长度。若它也超过 128 个 token，896/968 两桶就无法减少当前负载的 GEMM tile，后续设计会停在证据记录。 |
| 2026-09-14T22:59:56.221Z | backbone | L3184 | 真实快照确认了 seed 42 是 127 个语言 token、895 个有效 prefix 行，896 桶能少计算一个 M128 tile。官方 oracle 则是 903 行，会走 968 桶。两者必须分别验证，不能用其中一个长度代表动态任务输入。 |
| 2026-09-14T23:00:41.510Z | main | L5567 | 真实输入记录确认：seed 42 的有效 prefix 是 895 行，确实会使用 896 行范围；官方校验样本则有 903 行，会使用 968 行范围。因此，这项优化有当前负载上的机会，但验证必须覆盖长度切换和短范围路径。Softmax/PV 原型遇到了一处命名空间歧义，正在作最小构建修复，实验映射不变。  |
| 2026-09-14T23:03:19.028Z | ffn | L2717 | 接下来先准备静态短桶的收益上限筛选：在相同真实输入上比较 968 与 896 行的 51 次 GEMM，保留两路相同的 residual reset。当前只编写探针，等待 GPU 窗口。 |
| 2026-09-14T23:03:33.167Z | main | L5625 | Softmax/PV 融合通过了九组真实输入校验，但局部每次约慢 1.84 微秒，已停止。接下来先测试 padding 方案的静态收益上限：直接复用现有 GEMM，以 896 行处理真实输入；只有这一步显示足够收益，才实现运行时选择和对应接口。  |
| 2026-09-14T23:05:01.687Z | backbone | L3253 | 方案已收敛到显式传入现有 mask，先覆盖两处 FFN。需要同时保留旧控制方案的路由，并让短桶 GELU 直接写零到尾部；仅依赖 scratch 初始为零，无法保证长请求切换到短请求后一直有限。 |
| 2026-09-14T23:06:05.719Z | backbone | L3283 | 已提交 `da7599a`，仅文档和 CPU 证据。  - 两份真实 seed42 快照、共18个 mask 均为 **895有效行／127语言 token**，可选 M896；官方 oracle903行走M968。 - Target 可新增两处 FFN OpSpec，显式传入现有 mask；无需改共享 runner、host、固定KV布局或 capture政策。 - 必须保留旧 plan/control/reference 映射，并处理长→短切换后的 GELU 尾部有限性。 - 现有 Stream-K 无直接 device-M 支持；两桶 native 可行性由另一分析继续确认。  未运行 Torch、模型、GPU、JIT或编译。 |
| 2026-09-14T23:07:47.058Z | ffn | L2791 | 51 次真实 GEMM 的数值检查均通过。静态 896 行版本在本轮 ABBA 中平均省约 1.67 ms，高于约 0.29 ms 的对照漂移；这只表明 GEMM 部分有可测空间，尚未计入运行时选择和新增 launch 成本。GPU 已归还。 |
| 2026-09-14T23:08:20.628Z | main | L5724 | 896 行的静态筛选显示了足够空间：51 次 GEMM 的平均差约 1.67 毫秒，大于本轮约 0.29 毫秒的控制漂移，数值检查也通过了。这个结果尚未包含运行时选择和整模型访存成本；现在开始最小实现，再验证长度切换和完整部署延迟。  |
| 2026-09-14T23:09:01.680Z | backbone | L3297 | 静态 GEMM 结果支持继续做完整候选。我会先与 native 实现对齐 ABI，再接入显式 mask 和双计划 wrapper，并验证旧方案与 reference 的路由保持原来的 dense 行为。 |
| 2026-09-14T23:11:14.548Z | ffn | L2855 | 静态筛选结果已保存。现在仅实现两个 native 扩展：复用 cfg0 的双桶入口，以及短桶 GELU 的尾部零写；Python 和图接线由另一子任务负责。本轮不编译、不使用 GPU。 |
| 2026-09-14T23:13:02.703Z | main | L5851 | 实现将通过 Target 内部的显式 mask 参数选择计算范围；共享 runner 和 KV 物理布局可以沿用。正确性验证会专门覆盖 896 行边界，并在同一张已捕获的图上执行短→长→短，检查旧的 padding 值是否会影响后续请求。  |
| 2026-09-14T23:13:24.530Z | ffn | L2889 | 已提交 native 实现 `479a2d888604ed3b819ed968522e7078e97c4e85`，仅修改两个 CU 文件；ABI 已同步给 backbone 子任务。  静态筛选记录为 `5dd94fe`，测得约 **1.67 ms** 的 GEMM 收益空间。native 新入口尚未编译或验证，等待 GPU 授权。 |
| 2026-09-14T23:14:52.311Z | backbone | L3400 | CPU 路由检查已通过：旧完整方案仍走原 CUTLASS dense 路径，reference 仍走 torch，候选只改变两处 FFN backend。mask 已成为图中的显式输入；两个桶的计划和 workspace 都在 warmup 创建。接下来完成 Target 声明检查并提交。 |
| 2026-09-14T23:16:52.923Z | backbone | L3441 | 已提交 `6936cb3`，需配合 native 提交 `479a2d8`。  - 新增显式 mask、M896/M968 双计划 FFN backend。 - shipped 保持原 CUTLASS dense 路径；旧完整控制方案20条路由保持。 - CPU测试9项、Target声明8项及路由检查均通过。 - 未运行GPU、模型或编译；候选路由由主任务统一接入验证。 |
| 2026-09-14T23:17:07.610Z | main | L5966 | 候选代码已完成，CPU 声明和控制路由检查通过；默认部署仍保持原路径。新增边界校验暂时缺少官方推理所需的 Transformers 补丁，我正在找回原 oracle 的运行环境；候选的 GPU 编译和数值验证还未开始。  |
| 2026-09-14T23:19:49.840Z | backbone | L3452 | 我会让这两项测试显式指定 dense CUTLASS 控制方案，解除对默认 shipped 路由的依赖，然后仅运行这4项 CPU 测试。 |
| 2026-09-14T23:20:40.015Z | main | L6026 | 896 行边界的官方参考已生成，18 层 KV 和动作输出均为有限值，语言 token 数确认为 128。接下来编译新入口，先检查原控制路径在这个输入上的结果，再运行同一张图的短→长→短验证。  |
| 2026-09-14T23:28:50.197Z | main | L6094 | 同一张 CUDA Graph 的短→长→短官方对照已通过，动态长度切换和 18 层 KV 都通过检查。现在将两个 backbone FFN 路由切到候选实现，完成路由检查后连续运行一次 A-B-B-A 端到端对比；此前的局部收益还不能视为部署收益。  |
| 2026-09-14T23:31:20.055Z | main | L6153 | 023 的端到端收益已确认：控制组为 32.062/32.148 ms，候选为 30.316/30.315 ms，平均降低 1.789 ms，明显大于组内漂移，保留该实现。收益适用于有效前缀不超过 896 行的输入；长前缀正确性已通过，性能收益尚未测定。接下来保存结果，并查看新的 backbone 热点。  |
| 2026-09-14T23:31:51.374Z | ffn | L2902 | 我会只审查合并为单次 launch 的源码约束，重点看参数大小、`gridDim` 依赖，以及 Stream-K 的调度和归约语义。 |
| 2026-09-14T23:34:12.331Z | ffn | L2944 | 没有发现必须复制 mainloop 的语义障碍：当前执行路径不使用 `gridDim.x` 来划分工作，但必须按选中桶的原始 grid 边界早退。现有二进制显示两份参数合计不到 1 KiB，参数容量不是主要限制；主要风险是新增选择逻辑影响当前已用 254 个寄存器的 kernel。主任务测得未选桶开销合计仅约 83 µs，因此实现价值有限。 |
| 2026-09-14T23:34:53.916Z | main | L6237 | 新 profile 中，backbone GPU 时间约为 16.72 ms；未选中的 51 次 bucket 启动合计仅 0.083 ms。合并这些启动需要改动 Stream-K 参数选择，因此先停在 CPU 可行性分析。下一步筛选同一 padding 边界能否让 QKV 和输出投影获益。  |

</details>
