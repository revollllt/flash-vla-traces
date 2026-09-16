# 045 · 结合 Tensor 活跃度和同频吞吐，避免误读低 occupancy

结果：**调整优化优先级**。历史代码位置：`7085a4b 之后，011 阶段`。

将资源、活动率、频率与实际工作量一起解释；profile 负责定位，不替代部署测量。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

大 GEMM 优化后，backbone 仍是主要热点。高寄存器、单个驻留 CTA 和低 occupancy 看上去可能意味着继续调整并行度有空间。

## 当时看到了哪些证据

gate/up 为 254 registers、96 KiB dynamic shared、每 SM 一个 128-thread CTA。先选少量相关 NCU 指标，复用真实 layer0 输入和已部署库；测量采用 cache-control all、clock-control none、kernel replay，属于隔离的冷重放诊断。

## 提出了什么假设

需要区分“资源占用低”和“Tensor 单元闲置”。若 Tensor 已接近当前同频上限，单独追求 occupancy 不一定有价值；down 形状不同，值得单独采样而不是从 gate 类推。

## 执行了哪些命令、做了什么改动

运行 `lab.sm120.pi05_backbone_gate_ncu` 的输入准备和 profile 路径，以 NVTX 只选一个 GEMM；down 在测量区外恢复 residual。结合 Tensor/DRAM/L2、有效 SM 频率、实际工作量和 M tile padding 计算归一化吞吐。含凭据的历史 sudo 命令行已脱敏；不含凭据的采样参数和 CSV 保留。

## 得到了什么结果

gate Tensor 活跃度 92.06%，DRAM 14.63%，L2 49.03%；down 为 95.90%/20.36%/50.24%。down 在同次 2.725260 GHz 下有效工作吞吐为对应实测单元上限的 90.33%；将 M968 的实际 tile 计为 1024 行后为 95.56%，接近 Tensor 活跃度。

## 为什么保留、回退或继续调查

暂不重复同一轮 12-tile 搜索，将注意力转向其他形状与融合边界。数据支持当前大 GEMM 主要在做 Tensor 工作，但不证明所有优化空间耗尽。冷 replay 的 profiler duration 不能直接作为部署延迟，padding 差额也不是必然可回收的时间。

## 如何核对这段过程

本摘录有 167 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"agent_message": 16, "function_call": 18, "function_call_output": 18}`；这些数量不是独立任务数。我们不补写缺失原话。

- [results/rtx5090-pi05/gpt6-backbone-ncu-prep/README.md](evidence/results/rtx5090-pi05/gpt6-backbone-ncu-prep/README.md)

<details>
<summary>按时间查看当时公开的进展说明（原文，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T19:18:19.252Z | qkv | L512 | 我会使用仓库的 `ncu-report` skill 准备单次 gate GEMM 诊断，只做 CPU 查询和模块编写。重点区分 tensor 吞吐、内存带宽与调度限制，并明确缓存与未锁时钟的测量条件。  |
| 2026-09-14T19:22:45.893Z | qkv | L595 | 已确认 8 个指标在安装的 NCU 2026.2.1 / GB202 上可用，使用离线芯片查询完成，未运行 sudo 或 GPU workload。准备的模块先单独保存真实 layer 0 的归一化输入和已折叠权重；正式诊断只加载这两个张量与现有部署库，不触发编译。  |
| 2026-09-14T19:24:40.049Z | main | L1419 | 更新后的 GPU 时间线显示，backbone 仍占约 18.39 ms，是当前最大热点；expert 约 10.72 ms，视觉约 4.54 ms。expert 的 12 个 CUTLASS 配置均通过数值检查，但最佳配置的局部收益合计估计仅约 0.17 ms，先保留为候选，优先分析 backbone 的大 GEMM。  |
| 2026-09-14T19:25:12.416Z | qkv | L625 | NCU 准备已提交 `98bb052`。  - 模块：`lab/sm120/pi05_backbone_gate_ncu.py` - 完整命令：`results/rtx5090-pi05/gpt6-backbone-ncu-prep/README.md` - 已离线验证 8 个 GB202 指标。 - 使用现有部署库，50 次 warmup 后仅捕获一个 NVTX gate GEMM。 - 建议 `--cache-control all --clock-control none --launch-count 1`。  仅完成 CPU 检查；未运行 GPU workload、profiling、sudo 或重新编译。 |
| 2026-09-14T19:26:14.024Z | main | L1472 | 新的逐调用归因显示，backbone FFN 的 10.51 ms 中，10.03 ms 花在 34 次 gate/up GEMM，归一化和激活合计约 0.48 ms。接下来用已部署的实现和真实 layer-0 输入，采集单次 GEMM 的 8 项 NCU 指标；这次采样只用于判断瓶颈，性能结论仍以无 profiler 的端到端测量为准。  |
| 2026-09-14T19:28:21.574Z | backbone | L777 | 我会先核对 NCU 指标的准确含义和已有 tile 测量，把“资源占用低”与“吞吐仍有空间”分开判断。此次只做 CPU 分析，重点是选择下一次最便宜、能排除具体假设的实验。 |
| 2026-09-14T19:28:40.988Z | backbone | L787 | 我按 `ncu-report` 和 `hardware-unit-test` 核对了证据。此处的 8.33% occupancy 恰好对应每个 SM 的 4 个 active warps；已有 RTX 5090 微基准表明 4 个 warps 已能接近 BF16/FP32 累加的指令吞吐上限，因此不能据 occupancy 单独判断主循环缺少并行度。 |
| 2026-09-14T19:28:47.705Z | main | L1524 | NCU 显示，当前 gate GEMM 的 Tensor Core 活跃度约为 92.1%，DRAM 利用率约为 14.6%；低 occupancy 本身并不说明它有明显优化空间。接下来重点验证 expert GEMM 的 epilogue 能否融合门控残差，同时严格保留现有 BF16 中间舍入，减少一次 kernel 启动和临时张量读写。  |
| 2026-09-14T19:32:28.506Z | main | L1611 | 门控残差融合已有可行方案：让 CUTLASS 先完成归约，再显式舍入到 BF16，随后执行独立的 FP32 乘法和加法。另一个小候选是 action 输出头的逐元素融合，可将每步约 17 个 kernel 减到 3 个。两者都先做局部验证，再逐个测端到端收益。  |
| 2026-09-14T19:32:43.573Z | backbone | L819 | gate/up 的 tensor pipe 活跃度已达 92.1%，当前证据不支持优先通过增加 occupancy 获益。down 虽然 FLOP 数相同，但只有 128 个输出 tile，少于 170 个 SM，且 K 循环长 8 倍；值得对 down 单独做一次定向 NCU，确认 Stream-K 分工是否留下可改空间。 |
| 2026-09-14T19:33:51.142Z | backbone | L854 | 我会扩展现有 NCU 驱动，加入 down 的真实输入快照。down 使用 `beta=1`，每次 warmup 和最终测量前都会恢复同一份 residual；恢复操作放在 NVTX 测量区间外，保持各次调用的输入一致。 |
| 2026-09-14T19:34:45.852Z | main | L1653 | action 输出头候选已完成实现，开始测试真实 10 个去噪步骤。两侧计时都恢复相同的 action 初值，避免重复 Euler 更新影响比较。同时在准备 down GEMM 的单次 NCU 采样，并增加有效时钟指标，减少频率变化对吞吐判断的干扰。  |
| 2026-09-14T19:36:32.552Z | backbone | L918 | 已提交 `9bdd145`，分支 `gpt6-pi05-backbone-down-ncu`，起点 `63fa41a`。  - driver 新增 `--site down`，保存实际 layer 0 输入、权重和初始 residual。 - profile 使用 `beta=1, C=D`；50 次 warmup 和最终测量前均恢复 residual，reset 全在 NVTX 外。 - gate 默认参数及旧快照兼容。 - CPU 语法、CLI、旧快照读取检查通过；未运行 GPU、编译或 sudo。  root 接入后，prepare/profile 均追加 `--site down`，NVTX 改为 `pi05_backbone_down/`，快照和报告使用 `backbone-down-layer0`。原 8 指标追加 `sm__cycles_elapsed.avg.per_second`；名称已从本机 NCU 2026.2.1 的 `SpeedOfLight_RooflineChart.section:139` 离线确认。 |
| 2026-09-14T19:36:46.030Z | main | L1694 | action 输出头候选在真实 10 步输入上逐位一致，局部交错测量显示约 0.168 ms 的收益。这个幅度较小，我正在补跑当前部署版对照，再测候选版，以排除时钟或运行间漂移造成的假提升。  |
| 2026-09-14T19:42:07.676Z | main | L1847 | action 输出头的端到端收益已确认：交错测量中，对照版为 33.957/33.942 ms，候选版为 33.743/33.767 ms，约有 0.19 ms 的稳定收益；完整输出检查也通过。该改动已保留，接下来测试 expert down 的门控残差 epilogue。  |
| 2026-09-14T19:44:27.608Z | main | L1886 | expert down epilogue 的首次数值检查失败，误差远超正常归约差异，已停止测试且没有进入计时。该候选仍留在独立工作区，正在排查实现问题；当前部署版继续保持已验证的 33.74–33.77 ms 水平。  |
| 2026-09-14T19:48:24.841Z | main | L1958 | down GEMM 的采样同样显示较高的 Tensor Core 活跃度（95.9%）。按同次采样的有效时钟估算，有效计算吞吐约为对应上限的 90.3%，其中差距还包含填充和必要开销，因此暂不重复同一轮 tile 搜索。下一批筛选转向视觉 FFN 的不同矩阵形状，epilogue 候选则继续定向排错。  |

</details>
