# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/007.json](results/pi05-rtx5090/gpt6-run-01/measurements/007.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/007.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/008.json](results/pi05-rtx5090/gpt6-run-01/measurements/008.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/008.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/007-cutlass-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/007-cutlass-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/007-cutlass-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/008-cutlass-down-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/008-cutlass-down-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/008-cutlass-down-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/6a04c67.patch](patches/6a04c67.patch) | `git show 6a04c67 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；6a04c67db51b1582681c9f21be6cc06d900dc7fd Add Pi05 CUTLASS backbone GEMM candidate |
| [patches/54b5946.patch](patches/54b5946.patch) | `git show 54b5946 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；54b59465246eeb631eee006f810853a7f9e2baaa Deploy CUTLASS backbone down projection on RTX 5090 Pi0.5 |
