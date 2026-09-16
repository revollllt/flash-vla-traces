# 013 · Vision FFN 两处统一 cfg10，并保留部署漂移范围

[中文](case.md) | [English](case.en.md)

结果：**保留**。历史代码位置：`5f7ccf0`。

配置收益接近时优先复用一个已验证 tile，结论精度受对照漂移限制。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

Vision FFN 的两个 bias GEMM 在当前 profile 中仍有可测空间；需要验证不同形状是否能复用现有 CUTLASS 筛选接口。

## 当时看到了哪些证据

ldc=0 可在 FP32 accumulator 中广播 BF16 bias，再转 BF16；down 还需独立 BF16 residual add。两个站点分别使用最佳 cfg5/cfg10 只比统一 cfg10 额外估计快约 0.014 ms。首次误差汇总因约 8900 万元素触发 quantile 限制，不是 GEMM 数值失败。

## 提出了什么假设

两处共享一个 cfg10，可以用较小生产改动取得大部分局部收益；先逐层误差检查，再测完整 norm/GEMM/activation 或 residual 链。

## 执行了哪些命令、做了什么改动

使用 `lab.pi05.cutlass_vision_screen` 与 `lab.pi05.cutlass_vision_chain`，将误差汇总改为沿用现有函数逐层检查。选择 CTA64×128×32、warp32×64×32、5 stages，加入同一 native 库；保持 up 的 norm/GELU 和 down 的独立残差。真实 27 层与重复 replay 通过后，切换两处路由并做部署 ABBA。

## 得到了什么结果

编译为 160 registers、无 spills。局部两站点保守分离合计约 0.369 ms；官方 action cosine 0.9999915368、rel_rms 0.00411417。部署 A33.459378/33.579306 → B33.057243/33.062119 ms，A/B 漂移 0.119928/0.004875 ms。

## 为什么保留、回退或继续调查

保留：全部候选中位数仍低于对照；收益报告约 0.40–0.52 ms，而不是过度精确的单 kernel 因果增益。未因对照漂移继续追测，也不为局部额外约 0.014 ms 增加另一套生产模板。

## 如何核对这段过程

本摘录有 134 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"agent_message": 18, "function_call": 15, "function_call_output": 15}`；这些数量不是独立任务数。我们不补写缺失原话。

- [当时保存的实验总结](evidence/run-note.md)
- [全部证据与原始路径](evidence/README.md)

<a id="original-updates"></a>

<details>
<summary>按时间查看当时公开的进展说明（历史路径以代码显示，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T19:44:04.672Z | backbone | L929 | 我先核对现有 CUTLASS 接口能否保持 vision GEMM 的 bias 舍入顺序，再准备有限筛选。当前是 FP32 累加与 bias 相加后转 BF16；候选必须保留这个顺序，才能与部署实现直接比较。 |
| 2026-09-14T19:46:18.455Z | backbone | L970 | 现有实验库支持 `ldc=0`，可在 GEMM 的 FP32 epilogue 中直接广播 bias。main 的 cfg0 接口则固定 `C=D`；筛选时会把它每次复制 bias 的成本计入图内，并单独标识这条路径，避免把接口开销误归因于 tile。 |
| 2026-09-14T19:50:28.634Z | backbone | L1049 | 筛选脚本已完成 CPU 检查。现在开始运行两个 FFN GEMM 站点：每个图轮转 27 层真实权重，记录每个配置的 15 个原始样本，并用前后的 torch 测量检查漂移。 |
| 2026-09-14T19:51:58.249Z | backbone | L1081 | 首轮在误差汇总时触发了 `torch.quantile` 的输入大小限制：27 层 up 输出合并后约有 8900 万个元素。GEMM 已执行成功，尚无候选计时结论。我会保留失败日志，改为逐层使用现有误差函数，检查全部 27 层并报告最差结果。 |
| 2026-09-14T19:54:12.018Z | backbone | L1128 | 两个站点的 12 个配置都通过了全部 27 层数值检查。局部计时中，up 最佳配置约省 0.12 ms，down 最佳配置约省 0.26 ms，收益均大于前后 torch 测量的漂移。若两处统一使用 cfg10，局部收益合计约 0.36 ms，可作为较小的生产改动候选；部署收益仍需端到端验证。 |
| 2026-09-14T19:55:14.813Z | backbone | L1144 | GPU 已释放。提交：`a1c18a1` + `4b7dcef`，分支 `gpt6-pi05-vision-cutlass-screen`。  / 站点 / torch 前/后 / 最佳配置 / 候选延迟 / /---/---:/---:/---:/ / Up / 45.3547 / 45.5230 µs / cfg5 / 41.0335 µs / / Down / 47.7357 / 47.8021 µs / cfg10 / 38.1653 µs /  全部 12 配置、两个站点、各 27 层数值通过。控制漂移为 0.1683 / 0.0664 µs。  建议最小生产候选统一使用 cfg10：局部合计约省 **0.36 ms**；分别用 cfg5/cfg10 约省 **0.38 ms**。尚未验证部署收益。  原始样本、逐层误差及首次 quantile 失败日志均保留于 main 的 `artifacts/rtx5090-pi05/gpt6-cutlass-vision-screen*`；精简证据已写入 probe 文档。 |
| 2026-09-14T19:55:53.612Z | backbone | L1155 | 我会把 vision 的 cfg10 bias GEMM 加入现有 `cutlass_backbone.so`，复用当前 Target 的 norm/GELU。down 保持 GEMM 输出转 BF16 后再做 residual add；先确定 ABI 和 wrapper 结构，等共享库的新基点确定后再落代码。 |
| 2026-09-14T19:57:07.225Z | backbone | L1174 | 设计只涉及两个文件：  - `cutlass_backbone.cu`：追加 cfg10 的 `vision_bias_gemm_{workspace,plan,run,destroy}`。固定 `alpha=beta=1、ldc=0`，bias 在 FP32 accumulator 中相加后输出 BF16。 - `cutlass_vision.py`：懒加载同一原生库，实例持有 plans，Scratch 持有 workspace 和临时输出。  up：现有 norm → bias GEMM → 现有 GELU。   down：bias GEMM → BF16 临时输出 → 独立 `out.add_`。  等待 expert epilogue 合入后的基点，再创建干净实现分支；当前未改生产源码、未使用 GPU。 |
| 2026-09-14T19:58:00.524Z | backbone | L1185 | 我会从 `df996b8` 创建干净分支，按两文件方案实现 cfg10，并准备 27 层完整 FFN chain 的交错对比。也会核实筛选脚本的计时依赖，确保主分支上的 probe 可以复现。 |
| 2026-09-14T20:04:42.411Z | backbone | L1267 | 源码和完整 chain probe 已准备好。现在开始编译共享库中的 cfg10，并检查新增 wrapper 在无 CUDA 环境下能否完成声明。GPU 验证会先检查全部 27 层数值与重复回放一致性，通过后再运行 A/B/B/A 计时。 |
| 2026-09-14T20:07:45.878Z | backbone | L1330 | 两处完整 chain 均通过了 27 层数值检查，重复 CUDA Graph 回放也逐位一致。按较快的 A 与较慢的 B 保守比较，局部合计约省 0.369 ms；控制组漂移约 0.010–0.027 ms。GPU 已释放，结果可以交给主任务做官方精度与端到端验证。 |
| 2026-09-14T20:08:06.513Z | main | L2321 | 视觉 FFN 的统一配置已通过两处各 27 层的完整调用检查，重复 CUDA Graph 回放也逐位一致。局部交错测量显示约 0.37 ms 的保守收益，开始接入官方输出对齐和端到端验证；FFN down 仍保留独立残差加法，以维持原有舍入顺序。  |
| 2026-09-14T20:13:11.190Z | main | L2411 | 视觉 FFN 的完整模型对齐已通过，首轮端到端中位延迟为 33.057 ms，对照为 33.459 ms，正在完成交错复测。后续还准备了两个复用现有实现的小候选：expert 输出投影的门控残差融合，以及视觉 QKV 的同一 GEMM 配置。  |
| 2026-09-14T20:16:12.637Z | main | L2464 | 视觉 FFN 改动已保留：候选两次测量为 33.057/33.062 ms，对照为 33.459/33.579 ms。对照漂移约 0.120 ms，比此前大，因此暂将收益报告为约 0.40–0.52 ms。当前端到端中位延迟约 33.06 ms，较最初降低约 44.5%。  |

</details>
