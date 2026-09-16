# 005 · Expert attention 保留 FP32 scores，融合 scale/mask/softmax

[中文](case.md) | [English](case.en.md)

结果：**保留**。历史代码位置：`4d53ad7`。

保持关键数值边界与 alias 行为，同时给两路加入相同的输入恢复。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

Expert attention 的 Q/K 转换、FP32 SIMT GEMM 及逐元素 softmax 链合计约 5.623 ms、2160 次启动，是前一轮 profile 提出的具体热点。

## 当时看到了哪些证据

已安装 Torch 的接口支持 BF16 输入和 FP32 输出的矩阵乘；先在 GPU 确认这一实际能力。Pi0 的 mask 与 score 舍入不同，不能直接套用。原调用还允许 out=Q，重复回放会覆盖下一次输入。

## 提出了什么假设

使用 BF16 Tensor Core QK 输出 FP32 logits，合并 scale、运行时 additive mask、稳定 softmax，并在原边界写出 BF16 probabilities；PV GEMM 沿用原实现。

## 执行了哪些命令、做了什么改动

运行 `python -m lab.sm120.pi05_attention_fusion_probe`。首次编译缺少 math_constants.h，补 include 后继续。以实际形状合成输入检查不同 mask、输入尺度、out=Q 和 masked-V 独立性；局部两路都加入相同 Q reset。再集成 `80cf420`，以 `4d53ad7` 切换部署并做官方 parity。

## 得到了什么结果

masked V 改大不影响输出；局部检查通过现有容差，最坏输出 rel_rms 约 1.56e-4。含相同 reset 的局部链约 37.8 → 15.2 µs。真实权重官方对照通过，部署中位数 44.4914 → 41.0824 ms。

## 为什么保留、回退或继续调查

保留此 Expert attention 链。BF16 输入到 FP32 scores 的归约顺序可以变化，结果不是所有输入都逐位一致。此前默认 SDPA 的负结果属于另一 dispatch 筛选，不能用它否定这一实现或所有融合 attention。

## 如何核对这段过程

本摘录有 39 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"agent_message": 6, "function_call": 8, "function_call_output": 8}`；这些数量不是独立任务数。我们不补写缺失原话。

- [当时保存的实验总结](evidence/run-note.md)
- [全部证据与原始路径](evidence/README.md)

<a id="original-updates"></a>

<details>
<summary>按时间查看当时公开的进展说明（历史路径以代码显示，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T18:40:07.932Z | qkv | L193 | 继续处理 expert attention：先验证 BF16 输入、FP32 输出的矩阵乘路径是否可用，再融合 scale、mask 与 softmax。当前只准备代码，GPU 验证等待串行时段。  |
| 2026-09-14T18:46:02.775Z | qkv | L286 | Attention 候选已通过局部数值检查，运行时 mask 和 `out=Q` 别名行为正确。两次对照中位数约为 37.8 µs → 15.2 µs；计时两侧都重置 Q，避免重复回放改变输入分布。输出有极小舍入差异，最大相对 RMS 为 1.56×10⁻⁴，真实模型验证仍待集成后完成。  |
| 2026-09-14T18:46:55.636Z | qkv | L298 | Attention 候选已提交 `59fd90e`，可单独 cherry-pick。  - BF16 QK 输出 FP32，融合 scale/mask/softmax，保留 BF16 P@V。 - 合成输入局部计时：37.8 → 15.2 µs，约 **2.50×**。 - 最大相对 RMS 1.56×10⁻⁴；mask 与 `out=Q` 检查通过。 - 原始数据与命令在 `results/rtx5090-pi05/gpt6-attention/`。 - registry/target 未改；真实模型验证待主任务完成。  GPU 时段已释放。 |
| 2026-09-14T18:48:28.060Z | main | L626 | 打包专家 FFN 的部署收益也成立：**45.868→44.491 ms**，已保留。attention 候选在保留 FP32 分数和运行时 mask 的情况下，局部约 **37.8→15.2 µs**；接下来验证它的完整模型数值与端到端表现。大 GEMM 的 CUTLASS 筛选正在独占 GPU 串行运行。  |
| 2026-09-14T18:53:07.424Z | main | L701 | attention 的完整验证通过，部署中位数降至 **41.082 ms**，累计降低 **31.0%**。残差投影融合也在真实调用上逐位一致，并显示局部收益。五轮后热点占比已经变化，我会补一张当前部署版本的完整时间线，再继续集成。  |

</details>
