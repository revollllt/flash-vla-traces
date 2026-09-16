# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/001.json](results/pi05-rtx5090/gpt6-run-01/measurements/001.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/001.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/002.json](results/pi05-rtx5090/gpt6-run-01/measurements/002.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/002.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/002-qkv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/002-qkv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/002-qkv-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-qkv/README.md](results/rtx5090-pi05/gpt6-qkv/README.md) | `results/rtx5090-pi05/gpt6-qkv/README.md` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-qkv/local.json](results/rtx5090-pi05/gpt6-qkv/local.json) | `results/rtx5090-pi05/gpt6-qkv/local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/79a55c2.patch](patches/79a55c2.patch) | `git show 79a55c2 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；79a55c246289ed197d28f1f4cd7449a07be6cc2b Fuse Pi0.5 expert QKV pointwise stages on RTX 5090 |
| [patches/c8f5e15.patch](patches/c8f5e15.patch) | `git show c8f5e15 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；c8f5e1567ea90c1a0462dff9f237874e955d9407 Route RTX 5090 Pi0.5 expert QKV through native fusion |
