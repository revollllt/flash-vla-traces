# 009 · Vision LayerNorm/GELU 融合保留 centered variance 与舍入

[中文](case.md) | [English](case.en.md)

结果：**保留**。历史代码位置：`cc5f0a8`。

融合非线性之前先核对方差算法和 BF16 边界，不能只看公式名称。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

005 profile 中，vision norm_ffn_up 为 2.055 ms/270 launches，norm_qkv 为 1.249 ms/162 launches，均有重复转换和遍历。

## 当时看到了哪些证据

原 GEMM 已融合 bias；LayerNorm 用 FP32 均值和 centered variance，gamma/beta 在最终归一化 store 前保持 FP32。GELU 必须读已舍入为 BF16 的投影结果，不能提前进入 GEMM accumulator。

## 提出了什么假设

单个 CUDA pass 实现 FP32 LayerNorm→BF16，另一个原地 kernel 处理 BF16 投影后的 tanh GELU；保留两处 bias-fused torch.addmm。

## 执行了哪些命令、做了什么改动

运行 `python -m lab.sm120.pi05_vision_fusion_probe`，捕获 27 层真实输入并检查 0/13/26 层，在推进后层前恢复参考输出。计时两个完整调用链，保持 lazy native loader 和 runner scratch；集成后做官方 parity、CPU Target 声明和部署测量。

## 得到了什么结果

所选层的输出与归一化通过容差；局部 27 层 ABBA 合计约有 1.07 ms 空间。官方 action cosine 0.9999852922、rel_rms 0.00542377。部署中位数 36.2217 → 35.3236 ms，降低 0.8981 ms。

## 为什么保留、回退或继续调查

保留。两次部署的结束 SM/memory 时钟、温度和功耗限制原因相同，单次内部波动明显小于差距；这些终点观测不能证明完整频率轨迹相同，局部估计也不能取代部署值。

## 如何核对这段过程

本摘录有 65 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"function_call": 15, "function_call_output": 15, "agent_message": 11}`；这些数量不是独立任务数。我们不补写缺失原话。

- [当时保存的实验总结](evidence/run-note.md)
- [全部证据与原始路径](evidence/README.md)

<a id="original-updates"></a>

<details>
<summary>按时间查看当时公开的进展说明（历史路径以代码显示，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T18:55:09.654Z | qkv | L309 | 下一步准备 vision 的 LayerNorm 与 GELU 融合，保留 `torch.addmm` 和 BF16 预激活舍入。先复用现有实现与真实层输入做候选准备，编译和 GPU 测量继续串行安排。  |
| 2026-09-14T19:00:23.334Z | qkv | L397 | Vision 的真实 checkpoint 检查已通过：第 0、13、26 层的输出和归一化结果均满足现有容差。27 层局部图回放中，QKV 链路减少约 0.32 ms，FFN 链路减少约 0.75 ms；合计约 1.07 ms，接下来由主任务确认部署后的端到端收益。  |
| 2026-09-14T19:01:20.353Z | qkv | L409 | Vision 候选已提交 `fd207a7`，可单独 cherry-pick。  - 实际 checkpoint 第 0/13/26 层数值检查通过。 - 27 层局部计时：QKV 约 1.32 → 1.01 ms；FFN 约 2.18 → 1.43 ms。 - 合计局部减少约 **1.07 ms**；端到端收益待主任务验证。 - CPU 无 `CUDA_HOME` 的导入与 wrapper 构建通过。 - 记录在 `results/rtx5090-pi05/gpt6-vision/`，GPU 时段已释放。 |
| 2026-09-14T19:10:11.277Z | main | L964 | backbone down 的 CUTLASS 实现已通过官方输出对齐检查，端到端中位延迟从 37.033 ms 降到 36.222 ms。接下来分别接入视觉层融合和 backbone QKV 融合，并测试 expert GEMM 的候选配置；所有 GPU 测量继续串行执行。  |
| 2026-09-14T19:12:22.542Z | main | L1044 | 视觉融合的 CPU 路由检查已通过。expert CUTLASS 筛选在第一个配置执行时失败，目前还不能归因于 tile 或模型环境；正在用单个 GEMM 做短复现，确认原因后继续模型级测量。  |
| 2026-09-14T19:14:31.064Z | main | L1118 | 筛选已排除一个不值得实现的方向：视觉 residual 的加法只有约 1.47 µs/次，替换成自写加法不会减少 kernel 或内存访问，因此不新增代码。expert 的单独 GEMM 复现则成功了，原错误仍需排查。现在开始视觉融合的完整输出验证。  |
| 2026-09-14T19:17:01.797Z | main | L1184 | 视觉融合已通过完整对齐检查，端到端中位延迟降到 35.324 ms，比上一版减少 0.898 ms；两次测量结束时的时钟和温度一致。接下来验证 prefix QKV 融合。backbone attention 的分析也已完成：其逐元素部分已经融合，单独重写 softmax 的空间很小。  |

</details>
