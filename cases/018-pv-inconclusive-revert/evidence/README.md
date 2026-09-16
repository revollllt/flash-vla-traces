# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-018.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-018.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-018.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018.json](results/pi05-rtx5090/gpt6-run-01/measurements/018.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-018.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-018.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-018.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a1.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a1.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a2.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a2.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b1.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b1.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b2.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b2.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-decision.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-decision.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-decision.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/018-triton-pv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/018-triton-pv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/018-triton-pv-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/README.md](results/rtx5090-pi05/gpt6-attention-pv-triton/README.md) | `results/rtx5090-pi05/gpt6-attention-pv-triton/README.md` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/dispatch.json](results/rtx5090-pi05/gpt6-attention-pv-triton/dispatch.json) | `results/rtx5090-pi05/gpt6-attention-pv-triton/dispatch.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/local.json](results/rtx5090-pi05/gpt6-attention-pv-triton/local.json) | `results/rtx5090-pi05/gpt6-attention-pv-triton/local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/representative.json](results/rtx5090-pi05/gpt6-attention-pv-triton/representative.json) | `results/rtx5090-pi05/gpt6-attention-pv-triton/representative.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/7b1f6df.patch](patches/7b1f6df.patch) | `git show 7b1f6df -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；7b1f6dfb760333a5c350505b427c16eb300ff89e Add optional Pi0.5 attention backend with one PV launch |
| [patches/34add1b.patch](patches/34add1b.patch) | `git show 34add1b -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；34add1bc19f1b8bee46e10689e2b8a0af5363c7d Route expert attention through the single-launch PV backend |
| [patches/1542c05.patch](patches/1542c05.patch) | `git show 1542c05 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；1542c056405270475aff96bc9b7a3121f8058e18 Withdraw PV route after deployment timing remained sensitive to drift |
