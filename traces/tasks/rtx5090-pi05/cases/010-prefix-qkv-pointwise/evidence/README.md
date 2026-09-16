# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/009.json](results/pi05-rtx5090/gpt6-run-01/measurements/009.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/009.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010.json](results/pi05-rtx5090/gpt6-run-01/measurements/010.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010-prefix-qkv-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/010-prefix-qkv-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010-prefix-qkv-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/010-prefix-qkv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/010-prefix-qkv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/010-prefix-qkv-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/010-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/010-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/010-target-binding.txt` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/rtx5090_prefix_qkv.md](lab/pi05/rtx5090_prefix_qkv.md) | `lab/pi05/rtx5090_prefix_qkv.md` | git 21d95c3 的历史文件 |
| [patches/221de0c.patch](patches/221de0c.patch) | `git show 221de0c -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；221de0c711d8eb41b77313a4fa4a92c23df083e3 Fuse Pi0.5 RTX 5090 backbone QKV pointwise stages |
| [patches/7085a4b.patch](patches/7085a4b.patch) | `git show 7085a4b -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；7085a4ba9f9c3e3e31ce6cbe0f6db853f56ec6d3 Deploy fused prefix QKV on RTX 5090 Pi0.5 |
