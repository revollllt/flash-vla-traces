# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-outproj-epilogue-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-outproj-epilogue-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-outproj-epilogue-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-control-before-014.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-control-before-014.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-control-before-014.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014.json](results/pi05-rtx5090/gpt6-run-01/measurements/014.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-control-after-014.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-control-after-014.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-control-after-014.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/014-outproj-epilogue-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/014-outproj-epilogue-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/014-outproj-epilogue-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/rtx5090_expert_epilogue.md](lab/pi05/rtx5090_expert_epilogue.md) | `lab/pi05/rtx5090_expert_epilogue.md` | git 21d95c3 的历史文件 |
| [patches/9de2c9c.patch](patches/9de2c9c.patch) | `git show 9de2c9c -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；9de2c9cd72ddbfd596016939e6a68cbe0a97ee62 Reuse Pi0.5 gated epilogue for expert attention output |
| [patches/7669007.patch](patches/7669007.patch) | `git show 7669007 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；76690072d4c526bdabb807aa3434398a95a1477a Deploy shared rounded epilogue for expert output projection |
