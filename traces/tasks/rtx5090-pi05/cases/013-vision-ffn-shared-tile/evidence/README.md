# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/vision-cutlass-screen.json](results/pi05-rtx5090/gpt6-run-01/measurements/vision-cutlass-screen.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/vision-cutlass-screen.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-vision-chain-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-vision-chain-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-vision-chain-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-control-before-013.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-control-before-013.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-control-before-013.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013.json](results/pi05-rtx5090/gpt6-run-01/measurements/013.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-control-after-013.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-control-after-013.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-control-after-013.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/013-vision-cutlass-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/013-vision-cutlass-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/013-vision-cutlass-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/cutlass_vision_screen.md](lab/pi05/cutlass_vision_screen.md) | `lab/pi05/cutlass_vision_screen.md` | git 21d95c3 的历史文件 |
| [lab/pi05/cutlass_vision_chain.md](lab/pi05/cutlass_vision_chain.md) | `lab/pi05/cutlass_vision_chain.md` | git 21d95c3 的历史文件 |
| [patches/f22140f.patch](patches/f22140f.patch) | `git show f22140f -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；f22140fa759da074c34d5e4727525c6a2d24a4a9 Add cfg10 bias GEMM for Pi05 vision FFN chains |
| [patches/5f7ccf0.patch](patches/5f7ccf0.patch) | `git show 5f7ccf0 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；5f7ccf03a4f10853d719f01ec24946fc13357e78 Deploy common CUTLASS vision FFN tile on RTX 5090 Pi0.5 |
