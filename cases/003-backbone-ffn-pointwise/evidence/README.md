# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/002.json](results/pi05-rtx5090/gpt6-run-01/measurements/002.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/002.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/003.json](results/pi05-rtx5090/gpt6-run-01/measurements/003.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/003.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/003-backbone-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/003-backbone-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/003-backbone-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/003-backbone-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/003-backbone-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/003-backbone-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/0b062b3.patch](patches/0b062b3.patch) | `git show 0b062b3 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；0b062b37f286a3469ca977b100692963fac6a0a1 Fuse Pi05 RTX5090 backbone FFN pointwise stages |
| [patches/571b9b4.patch](patches/571b9b4.patch) | `git show 571b9b4 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；571b9b4fc5177dac96a0d8404b14195ef8ddcff6 Route RTX 5090 Pi0.5 backbone FFN through native fusion |
