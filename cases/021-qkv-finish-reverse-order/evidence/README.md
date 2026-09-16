# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-local-qkv-finish.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-local-qkv-finish.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-local-qkv-finish.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020-control-before-021.json](results/pi05-rtx5090/gpt6-run-01/measurements/020-control-before-021.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020-control-before-021.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021.json](results/pi05-rtx5090/gpt6-run-01/measurements/021.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020-control-after-021.json](results/pi05-rtx5090/gpt6-run-01/measurements/020-control-after-021.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020-control-after-021.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a1.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a1.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a2.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a2.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b1.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b1.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b2.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b2.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-decision.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-decision.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-decision.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/021-qkv-finish-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/021-qkv-finish-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/021-qkv-finish-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/expert_qkv_finish_screen.md](lab/pi05/expert_qkv_finish_screen.md) | `lab/pi05/expert_qkv_finish_screen.md` | git 21d95c3 的历史文件 |
| [patches/c070e92.patch](patches/c070e92.patch) | `git show c070e92 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；c070e924829e1e7a6d18088cf8006d517d7e8b33 perf(pi05): add rounded QKV GEMM epilogue fusion backend |
| [patches/4de57c9.patch](patches/4de57c9.patch) | `git show 4de57c9 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；4de57c91b0ce1e22ad8350836f2ca3f292e1e90e Route expert QKV through the rounded GEMM epilogue fusion |
