# 017 · Vision 输出投影复用 cfg10，保留独立 BF16 residual add

[中文](case.md) | [English](case.en.md)

结果：**保留**。历史代码位置：`eb8c16a`。

复用与融合是不同假设；保持原舍入顺序后独立验证新形状的性能。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

Vision 输出投影的 GEMM 与 residual add 仍有局部优化空间，已部署 cfg10 可用于这一较小的 M768/K1152/N1152 形状。

## 当时看到了哪些证据

与 Vision FFN down 的主要区别是 K；现有 residual wrapper 可从权重 shape 获得 K。当前语义要求 accumulator+bias 转 BF16 后，才做独立 residual add。该站点共有 27 层，权重总量 68.34375 MiB，可装入 L2。

## 提出了什么假设

复用 cfg10 GEMM 和现有 wrapper，以不新增 native kernel/ABI 的方式降低 GEMM 时间；本轮不融合 residual，以保持原来的两次舍入。

## 执行了哪些命令、做了什么改动

以 `lab.pi05.cutlass_vision_chain` 选择 vision output 站点，检查全部 27 层输出和重复 replay，再做完整链 ABBA。集成 `d1b6f99` 并以 `eb8c16a` 仅切换这一处部署路由，运行官方 parity 与四个独立进程测量。

## 得到了什么结果

27 层通过现有容差，最坏 rel_rms 0.00105418；不与 cuBLAS 逐位相同，但候选重复回放一致。局部 A0.587776/B0.454656/B0.454656/A0.587776 ms。部署 A32.771943/32.782361 → B32.610786/32.613452 ms，平均改善 0.165034 ms。

## 为什么保留、回退或继续调查

官方对照通过，部署 A/B 漂移 0.010418/0.002666 ms，故保留。局部权重集可驻留 L2，部署结果另行测量；通过容差与重复回放一致不能写成与参考逐位一致。

## 如何核对这段过程

本摘录有 48 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"agent_message": 8, "function_call": 7, "function_call_output": 6}`；这些数量不是独立任务数。我们不补写缺失原话。

- [当时保存的实验总结](evidence/run-note.md)
- [全部证据与原始路径](evidence/README.md)

<a id="original-updates"></a>

<details>
<summary>按时间查看当时公开的进展说明（历史路径以代码显示，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T20:33:08.929Z | ffn | L1236 | 我先核对 vision out-projection 的形状、舍入顺序和已有 trace，评估复用 cfg10 的空间；这一步只读源码和记录。  |
| 2026-09-14T20:34:43.459Z | ffn | L1270 | 源码允许直接复用现有 bias GEMM：out-projection 与 FFN down 的差别主要是 K 从 4304 变为 1152。当前 trace 中它已经只有 GEMM 和 residual add 两个 kernel，因此收益取决于 cfg10 在这个较小 GEMM 上是否更快。  |
| 2026-09-14T20:35:48.917Z | ffn | L1291 | 可以复用 cfg10，无需修改 native kernel 或 ABI。  - 形状：`M768 K1152 N1152`，BF16 bias；`res=out`。 - 保留两次舍入：GEMM+bias→BF16，再 residual add→BF16。 - 010 trace：GEMM `476.274 μs / 27次`，add `40.443 μs`，总 `0.516717 ms`。 - 最小改动：扩展现有 vision wrapper，将 residual 路径的 K 参数改为取自 weight。  收益尚不确定：cfg10 仅 108 个输出 tile，权重总量 68.34 MiB 可落入 L2。值得做一次局部 ABBA 筛选；未实现、编译或运行 GPU。 |
| 2026-09-14T20:36:22.147Z | ffn | L1304 | 我会从包含当前 ABI 的新基点准备候选，只复用 cfg10 并保留现有 BF16 residual add。局部探针只测这个 site 的 27 层，等待明确授权后再使用 GPU。  |
| 2026-09-14T20:38:10.119Z | ffn | L1335 | 准备完成，分支 `gpt6-pi05-vision-outproj`，基点 `7b2d9d0`。  仅改两个文件：复用现有 cfg10 residual wrapper，并增加 `--site out` 的 27 层数值检查和 ABBA。保留 BF16 投影、原 residual add 和 `res=out`。  语法及 diff 检查通过；未改路由、未编译、未运行 GPU，等待明确时隙。 |
| 2026-09-14T20:38:34.935Z | ffn | L1346 | 开始验证现有 cfg10 的 vision out-projection：先检查 27 层数值和重复回放，再做局部 ABBA。若收益没有超过对照漂移，会记录为未通过并撤回 wrapper 改动。  |
| 2026-09-14T20:39:39.512Z | ffn | L1369 | 27 层数值检查和重复回放均通过。局部 ABBA 的两个对照中位数相同，候选都快约 0.133 ms，明显超过样本波动；我会保留这个候选并提交，部署收益继续由主任务验证。  |
| 2026-09-14T20:50:33.991Z | main | L3175 | 视觉输出投影候选已接入验证，复用现有 CUTLASS tile，并保留投影后再加残差的两次 BF16 舍入。它没有新增 native kernel；本轮仍只切换这一个部署位置，验证局部约 0.133 ms 的收益能保留多少。  |
| 2026-09-14T20:52:41.792Z | main | L3225 | 视觉输出投影已通过官方检查，首轮端到端从对照的 32.772 ms 降到 32.611 ms，正在完成后两轮。后续 QKV 实验会显式控制缓存：它的 18 层权重接近 L2 容量，单纯循环局部调用可能高估部署收益。  |
| 2026-09-14T20:54:11.723Z | main | L3257 | 视觉输出投影收益已确认：四轮对照支持约 0.165 ms 的端到端改善，候选两次中位数为 32.611 / 32.613 ms。当前固定工作负载已从最初 59.578 ms 降到约 32.61 ms，累计约 45.3%；继续验证 PV 与 QKV 的局部候选。  |

</details>
