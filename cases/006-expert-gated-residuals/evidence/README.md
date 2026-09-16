# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/005.json](results/pi05-rtx5090/gpt6-run-01/measurements/005.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/005.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/006.json](results/pi05-rtx5090/gpt6-run-01/measurements/006.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/006.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/006-residual-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/006-residual-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/006-residual-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/006-residual-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/006-residual-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/006-residual-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/006-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/006-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/006-target-binding.txt` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/rtx5090_fused_residual.md](lab/pi05/rtx5090_fused_residual.md) | `lab/pi05/rtx5090_fused_residual.md` | git 21d95c3 的历史文件 |
| [patches/da1ebf5.patch](patches/da1ebf5.patch) | `git show da1ebf5 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；da1ebf57cbe36bb2105515b134dba7930a7957a5 Fuse Pi0.5 RTX 5090 expert gated residual updates |
| [patches/e98b74c.patch](patches/e98b74c.patch) | `git show e98b74c -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；e98b74c0711ea75fe658e01101c8a27a1e27b2af Deploy fused gated residuals on RTX 5090 Pi0.5 |
| [patches/e9875fa.patch](patches/e9875fa.patch) | `git show e9875fa -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；e9875fa1be420cdb4464c3d4c4e0ead4e73ef381 Keep native backbone loading lazy and record residual deployment gain |
