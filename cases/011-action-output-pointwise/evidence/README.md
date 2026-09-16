# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010-control-before-011.json](results/pi05-rtx5090/gpt6-run-01/measurements/010-control-before-011.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010-control-before-011.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011.json](results/pi05-rtx5090/gpt6-run-01/measurements/011.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/011-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010-control-after-011.json](results/pi05-rtx5090/gpt6-run-01/measurements/010-control-after-011.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010-control-after-011.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/011-action-out-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/011-action-out-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/011-action-out-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/011-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/011-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/011-target-binding.txt` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-action-out/README.md](results/rtx5090-pi05/gpt6-action-out/README.md) | `results/rtx5090-pi05/gpt6-action-out/README.md` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-action-out/local.json](results/rtx5090-pi05/gpt6-action-out/local.json) | `results/rtx5090-pi05/gpt6-action-out/local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/fc73104.patch](patches/fc73104.patch) | `git show fc73104 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；fc731043760d885117b14b882e9719bc3d19cf3b Fuse Pi0.5 action output normalization and Euler update |
| [patches/853d1fa.patch](patches/853d1fa.patch) | `git show 853d1fa -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；853d1fa9e12ef620fde30b774f21f32103fc28e9 Deploy fused action output pointwise stages on RTX 5090 Pi0.5 |
