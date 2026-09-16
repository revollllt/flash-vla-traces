# 026 · 读源码与已有 trace，提前否决没有减少工作量的融合

结果：**CPU 筛选停止**。历史代码位置：`005 部署后的已有 profile`。

先确认现有实现到底做了什么；无需 GPU 实验也能排除无收益的重写。

本案例是 2026-09-16 的事后整理。下文的假设依据当时可见说明与实验文档；它不是模型内部思维记录。原话、命令和工具返回见 [trace.jsonl](trace.jsonl)，数据与实际补丁见 [证据目录](evidence/README.md)。共同条件见 [来源说明](../../README.md#来源与边界)。

## 遇到了什么瓶颈

Vision 的 out projection 与 FFN down 各有三个 launch，看上去可能存在 bias/residual 的逐元素融合机会。

## 当时看到了哪些证据

源码实际是 out.add_(torch.addmm(bias, x, weight))。已有 005 profile 的调用序列为 864-byte memset、GEMM、BF16 residual add，没有独立 bias copy/add。27 层两处 residual add 合计仅 0.079933 ms。

## 提出了什么假设

待检验的问题是“三个 launch 中是否存在可合并的独立 bias 运算”。如果 bias 已在 GEMM 内，单独新写一个 residual kernel 不会减少启动或张量遍历。

## 执行了哪些命令、做了什么改动

读取 torch_ops 和现有 vision profile，按调用顺序、graph node id、grid 将共享 kernel 名字映射回两个调用点。没有新增 CUDA 实现、编译、模型加载或 GPU 对照。原 profile 和当时分析说明均附带，命令保留在 trace。

## 得到了什么结果

每个序列只有一个独立 residual add；bias 已包含在 GEMM 路径内。将 GEMM 先单独舍入再合 bias/residual 还会增加原语义不存在的舍入边界。独立 residual 实现可删除的启动数和遍历数均为零。

## 为什么保留、回退或继续调查

在 CPU 筛选阶段停止这一具体方案。更广的 GEMM epilogue 融合仍是另一项可检验假设，需保留 accumulator+bias→BF16→residual 的舍入位置。小 memset 用于 scratch 初始化是推断，byte 数与持续时间才是直接观测。

## 如何核对这段过程

本摘录有 19 条可见事件，按 UTC 时间排列；原始文件名和一基行号保留在每条 `source` 中。父任务窗口包含并行候选的上下文，不应把窗口中的每条命令都归因于本案例。工具调用与返回按 `call_id` 配对；原平台已经截断的返回仍保持原样。

子任务的任务指令与回报存在加密 payload，因此无法完整恢复委派 prompt。该窗口未导出的通信/编排记录数量为 `{"function_call": 3, "function_call_output": 3, "agent_message": 3}`；这些数量不是独立任务数。我们不补写缺失原话。

- [results/rtx5090-pi05/gpt6-vision-residual-screen/README.md](evidence/results/rtx5090-pi05/gpt6-vision-residual-screen/README.md)

<details>
<summary>按时间查看当时公开的进展说明（原文，可能含相邻任务）</summary>

| UTC 时间 | 来源 | 原行号 | 当时的公开说明 |
|---|---|---|---|
| 2026-09-14T19:12:00.771Z | qkv | L420 | 先检查 vision 残差投影的三个 launch 分别在做什么，重点确认 bias 与 residual 之间的 BF16 舍入边界。只有能保留这些边界、且预计收益超过噪声时，才准备最小融合方案。  |
| 2026-09-14T19:14:18.022Z | qkv | L487 | 这条融合假设已被现有 trace 排除：bias 已在 GEMM 内处理，剩余残差加法每次只有约 1.47 µs。换成独立 CUDA 加法既不能减少 launch，也不能减少遍历；要消除它必须改 GEMM epilogue，并保留两次 BF16 舍入，因此本轮不增加无收益 kernel。  |
| 2026-09-14T19:15:02.678Z | qkv | L501 | 已提交负面结果 `7a3cbc3`，仅增加简短说明。  已有 trace 表明 bias 已融合进 GEMM；残差加法合计仅 **0.079933 ms**。独立 native 加法无法减少 launch 或流量，因此未实现、未运行 GPU 或 ABBA。  说明位于 `results/rtx5090-pi05/gpt6-vision-residual-screen/README.md`。工作区干净，等待下一热点。 |

</details>
