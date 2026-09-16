# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-control-before-020.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-control-before-020.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-control-before-020.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020.json](results/pi05-rtx5090/gpt6-run-01/measurements/020.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/020-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-control-after-020.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-control-after-020.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-control-after-020.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/020-triton-qk-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/020-triton-qk-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/020-triton-qk-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/README.md](results/rtx5090-pi05/gpt6-attention-qk-triton/README.md) | `results/rtx5090-pi05/gpt6-attention-qk-triton/README.md` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/local.json](results/rtx5090-pi05/gpt6-attention-qk-triton/local.json) | `results/rtx5090-pi05/gpt6-attention-qk-triton/local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/representative.json](results/rtx5090-pi05/gpt6-attention-qk-triton/representative.json) | `results/rtx5090-pi05/gpt6-attention-qk-triton/representative.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/dispatch.json](results/rtx5090-pi05/gpt6-attention-qk-triton/dispatch.json) | `results/rtx5090-pi05/gpt6-attention-qk-triton/dispatch.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/0c20f24.patch](patches/0c20f24.patch) | `git show 0c20f24 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；0c20f245be406c3e40f33c6b343b65272e5dd388 Add optional Pi0.5 attention backend with fixed FP32 QK |
| [patches/76086d9.patch](patches/76086d9.patch) | `git show 76086d9 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；76086d9564d4083860cf3335843742e3ad9f5b87 Route expert QK through the measured FP32-score tile |
