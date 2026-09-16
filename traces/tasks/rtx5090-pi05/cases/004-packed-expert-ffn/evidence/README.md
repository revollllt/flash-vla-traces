# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/003.json](results/pi05-rtx5090/gpt6-run-01/measurements/003.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/003.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/004.json](results/pi05-rtx5090/gpt6-run-01/measurements/004.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/004.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/004-packed-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/004-packed-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/004-packed-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/004-packed-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/004-packed-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/004-packed-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/622f161.patch](patches/622f161.patch) | `git show 622f161 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；622f1611954e3e38ca7878d6eafc48d52e6a82f5 Pack Pi0.5 RTX 5090 expert gate and up into one GEMM |
| [patches/ebfde75.patch](patches/ebfde75.patch) | `git show ebfde75 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；ebfde7594551bde089d81da3954e57a4dc61e18a Deploy packed expert FFN on RTX 5090 Pi0.5 |
