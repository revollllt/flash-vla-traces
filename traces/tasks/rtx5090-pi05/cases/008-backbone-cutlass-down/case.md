# 008 · 单独接入 Backbone down，保留 C=D 的残差语义

[中文](case.md) | [English](case.en.md)

结果：**保留**。历史代码位置：`54b5946`。

同一底层候选的不同调用点分别部署，避免把累计收益重复计数。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

案例 007 已接入 gate/up，但 backbone down 仍使用原 GEMM，真实局部总时间约 5.8 ms。

## 当时看到了哪些证据

准备 cfg0 时已检查 down 的真实 17 层调用。它与 Expert down 不同：原 residual 位于 GEMM epilogue，要求 alpha=beta=1 和 C=D，而不是 beta=0 后再做门控。

## 提出了什么假设

将已验证的 cfg0 Stream-K tile 用于 down，在保留原残差表达式的情况下获得独立增量收益。

## 执行了哪些命令、做了什么改动

复用案例 007 的 cutlass_backbone 实现和真实调用探针；workspace 与绑定指针的 plan 归各 runner 所有，在 warmup 初始化。以 `54b5946` 单独改 down 的部署路由，再运行官方 parity 与独立进程延迟命令。实现和路由补丁、局部完整报告均附带。

## 得到了什么结果

17 次实际调用通过既有容差，局部总时间 A5.827/5.869 → B5.038/5.039 ms。官方检查通过，部署中位数 37.0332 → 36.2217 ms。

## 为什么保留、回退或继续调查

保留，这是已保留 007 之后的增量，不再以 006 或最初模型为对照重复计算。当前结果只支持该工作负载、该 residual 表达式和 cfg0 调度。

## 如何核对这段过程

本摘录有 41 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"agent_message": 4, "function_call": 5, "function_call_output": 5}`；这些数量不是独立任务数。我们不补写缺失原话。

- [当时保存的实验总结](evidence/run-note.md)
- [全部证据与原始路径](evidence/README.md)

<a id="original-updates"></a>

<details>
<summary>按时间查看当时公开的进展说明（历史路径以代码显示，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T18:51:53.910Z | backbone | L335 | 我会先实现只含配置 0 的 Pi0.5 CUTLASS 路径，分别提供 gated FFN 和 down residual 两个可独立路由的调用点。已有 pointwise 路径保持不变；新的 native plan 与 workspace 由当前 wrapper 实例和 runner scratch 持有，编译和 GPU 验证等独占时段。 |
| 2026-09-14T18:56:26.077Z | backbone | L378 | 实现已限定为一个 CUTLASS tile，未改动原有 fused-backbone 路径。另外发现两个本机 CUTLASS checkout 版本不同，首轮验证会沿用此前筛选所用的主项目 vendored 版本，避免把库版本变化混入候选收益。 |
| 2026-09-14T18:58:13.335Z | backbone | L408 | 已开始独占编译和验证。这次先让真实模型完成 warmup 与 CUDA Graph capture，检查 native plan 和共享 workspace 能否稳定重放，再分别比较 FFN 与 down 的实际调用耗时，确认筛选收益能转移到部署绑定。 |
| 2026-09-14T19:01:00.435Z | backbone | L461 | 真实模型已成功捕获，重复部署重放输出完全一致。17 次 FFN 局部耗时约由 `11.59–11.86 ms` 降至 `10.87–10.88 ms`，17 次 down 由 `5.83–5.87 ms` 降至 `5.038–5.039 ms`，数值检查全部通过。  主任务发现的 CPU declaration 问题也已纳入：所有 native 加载延迟到首次实际调用；分别路由 FFN、down、两者的 declaration 检查均通过，CUDA 始终未初始化。 |
| 2026-09-14T19:01:34.919Z | backbone | L475 | 已提交 `d7f89142e2507dc7b649bdfac44677dbf2c5a9ef`，仅新增 `cutlass_backbone.py/.cu`；两个调用点可独立路由。  - FFN：17 次调用 `11.59–11.86 → 10.87–10.88 ms`。 - Down：17 次调用 `5.83–5.87 → 5.038–5.039 ms`。 - 34 个真实调用数值通过，模型重复重放完全一致。 - 三种路由的 CPU declaration 检查通过，未初始化 CUDA。  Native 加载已延迟；workspace 和 plan 均按实例持有。原始证据为 `artifacts/rtx5090-pi05/gpt6-cutlass-backbone-local.json`。GPU 已释放，端到端收益待主任务串行验证。 |
| 2026-09-14T19:06:06.843Z | main | L937 | CUTLASS 的主干 gate/up 改动已获得部署收益：**37.981→37.033 ms**，官方比较通过。下投影正在单独测量；当前保留的代码继续使用 BF16 输入/输出和原有数值容差，没有改去噪步数或模型规模。  |
| 2026-09-14T19:10:11.277Z | main | L964 | backbone down 的 CUTLASS 实现已通过官方输出对齐检查，端到端中位延迟从 37.033 ms 降到 36.222 ms。接下来分别接入视觉层融合和 backbone QKV 融合，并测试 expert GEMM 的候选配置；所有 GPU 测量继续串行执行。  |

</details>
