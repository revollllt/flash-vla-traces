# Flash-VLA Traces

[中文](README.md) | [English](README.en.md)

记录优化中的证据、尝试与决策，供其他模型学习问题推进方式。

当前内容：[45 个案例的索引](index.md)，其中 26 个具有中英文六步案例、按时间排序的可见轨迹和关键证据。推荐先看 [001 基础融合](cases/001-expert-ffn-fusion/case.md)、[026 提前筛选](cases/026-vision-residual-source-screen/case.md)、[018 不确定而撤回](cases/018-pv-inconclusive-revert/case.md) 与 [021 复测后保留](cases/021-qkv-finish-reverse-order/case.md)。

本次新增 14 个案例，补齐除 023/024 外的全部 20 个保留优化。原有 023 案例保留并翻译，024 不在本次展开范围内；其余筛选和诊断案例沿用原有整理范围。

## 文件组织

```text
index.md / index.en.md
cases/<编号>-<主题>/
  case.md / case.en.md    # 中英文六步案例，逐节对应
  trace.jsonl             # 当时公开说明、工具调用、工具返回
  evidence/README.md / README.en.md  # 双语证据来源入口
  evidence/…              # 原测量、失败结果、历史补丁、实验文档
experiments/README.md / README.en.md  # 双语学生实验使用约定
```

中文默认文件名不变，英文使用 `.en.md`，页首互相链接。案例六步正文、索引、仓库说明及证据指南提供中英文版本；trace、命令、测量、补丁、历史公开说明和归档实验文档共用原文。英文案例链接到原始公开说明附录，不把翻译写入原始事件。

每个案例按“瓶颈 → 当时证据 → 假设 → 命令与改动 → 结果 → 保留/回退/继续调查”组织。case.md 是回顾性叙述；trace.jsonl 是保留顺序的原始可见事件摘录。实验文档可能在实验后追加结果，不能把文档里的结论当成 agent 事前已知信息。时间戳用于核对公开说明和工具操作的先后关系。

## 来源与边界

来源任务：**优化 RTX 5090 上 Pi0.5 延迟**。模型为 GPT-6 Astra（gpt-6-astra），reasoning effort 为 xhigh。原实验是普通优化，没有额外要求记录教学 trace。本仓库是后续整理结果。

主要可见执行时间：2026-09-15 02:20–08:18（Asia/Shanghai），trace 内使用原始 UTC。代码从 5ac75bc 开始；保存资料时读取的已提交历史为 21d95c3。末段会话因 429 重试上限中止，不代表已证明优化空间耗尽。

工作负载：kai0/pi05-belt-cup/orbax-39999+openpi-convert-pi05_aloha，BF16、batch1、3×224×224 图像、200 prompt slots、chunk50、18 层、10 denoise steps。延迟 seed42；原官方对照是单独保存的 seed0 fixture。RTX5090、170 SM、96 MiB L2，driver580.142、torch2.13.0+cu130、Triton3.7.1、native CUDA13.1，未锁频。

部署测量使用初次 capture 后的 5 warmup / 100 samples，每版本独立进程、同一 GPU 串行、无 profiler。scope 包含输入 staging、host 处理、graph replay 与最终同步，不含模型加载/capture。后期小收益试验使用 ABBA，有些增加了事前限定的 BAAB；不能把后期协议追溯成早期也执行过。局部实验的 warmup/repeat/cache/reset 以其原记录为准。

该轮记录的初始/最后保留测量点为 59.577903 / 30.185328 ms。它们是固定工作负载下的一次累积优化轨迹，不是对其他开源项目的公平排名，也不是机器人任务成功率证据。官方 oracle 来源包含带本地修改的 vendored OpenPI；初始“torch”路线也不意味着所有内部计算都未编译。

原始会话仍保留在本机私有 `/Users/zou/.codex/sessions/2026/09/15/`，不复制进本仓库：

| 角色 | Session ID | 原文件 |
|---|---|---|
| main | `01a0a125-31f9-7be2-af47-1e480f38ca2d` | `rollout-2026-09-15T02-19-24-01a0a125-31f9-7be2-af47-1e480f38ca2d.jsonl` |
| ffn | `01a0a12d-5cb3-7a50-ba9a-f2075d1006c2` | `rollout-2026-09-15T02-28-19-01a0a12d-5cb3-7a50-ba9a-f2075d1006c2.jsonl` |
| qkv | `01a0a12d-af7f-7263-a778-f7c0dd627db9` | `rollout-2026-09-15T02-28-40-01a0a12d-af7f-7263-a778-f7c0dd627db9.jsonl` |
| backbone | `01a0a131-ce71-7062-bbb5-d4f41ff2d2cd` | `rollout-2026-09-15T02-33-11-01a0a131-ce71-7062-bbb5-d4f41ff2d2cd.jsonl` |

源代码/结果来自 `yx5090:/home/ubuntu/flash-vla`，当前机器工作树已有后续改动，整理时没有修改它。证据说明列出各文件原路径；实现补丁从相应历史提交提取，未从今天的工作树重建。没有新运行 GPU 优化或重测。

原始会话含敏感输入、加密通信与内部状态。本库仅导出 assistant 的公开 commentary/final 文本和执行工具的输入/输出；不导出用户原始输入、内部 reasoning、加密 agent payload 或 compacted state。凭据及含 sudo stdin 凭据的整行做脱敏。部分子 agent 委派指令不可读，不能据此声称保存了完整的分工 prompt。整理中的自然语言推断是注释，不是复原的内部思维。

每条 trace 记录包含 `timestamp`、`source.agent/session_id/file/line`、`kind`；工具事件另有 `call_id`。父子任务各取自己的执行窗口，排除继承的早期上下文；同一原事件可能被多个相关案例引用，跨案例合并时按 session_id+line 去重。并行窗口保留相邻任务上下文，跨任务时间戳本身不建立因果关系。工具返回若已被平台截断，不补写缺失内容。

附带材料足以审阅本案例报告的关键数值和主要代码改动；不构成脱离源项目即可运行的环境包。权重、原始输入 safetensors、部分二进制及完整构建依赖未复制。原文历史路径/链接保留作出处，可能无法在本仓库直接打开。精确重跑需要源项目相应提交及数据。

## 维护与使用

这是独立的本地 Git 仓库，暂未添加远程或 submodule。Flash-VLA 负责实现，本库负责案例。需要可执行复现时，再由本库引入对应版本的 Flash-VLA；默认不把案例仓库挂到所有学生工作区，以免无 trace 对照组通过搜索读到材料。

案例中原来的 GPU 时隙协调、暂停和授权字样属于历史记录，不是给读者的新指令。学生应提出自己的候选，并按当前工作负载重新验证；不得直接继承案例的速度结论。研究对照的最小约定见 [experiments/README.md](experiments/README.md)。

当前仍有 19 项仅列索引（含本次排除的 024），不宣称已经完成全部 45 个案例。案例内容及数据尚未对外发布；对外发布时需另行确定许可与引用方式。
