# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-control-before-015.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-control-before-015.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-control-before-015.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015.json](results/pi05-rtx5090/gpt6-run-01/measurements/015.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/015-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-control-after-015.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-control-after-015.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-control-after-015.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/015-vision-qkv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/015-vision-qkv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/015-vision-qkv-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md](results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md) | `results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md` | 实验保存的结果文件（2026-09-16 读取） |
| [results/rtx5090-pi05/gpt6-vision-qkv-cfg10/local.json](results/rtx5090-pi05/gpt6-vision-qkv-cfg10/local.json) | `results/rtx5090-pi05/gpt6-vision-qkv-cfg10/local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/9215399.patch](patches/9215399.patch) | `git show 9215399 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；9215399dae787fea819d95d1f89c0a850bf0e429 Reuse the vision cfg10 bias GEMM for QKV projection |
| [patches/5772cf2.patch](patches/5772cf2.patch) | `git show 5772cf2 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；5772cf204fe8ffc752e6463eb2eca21a5768e79d Route vision QKV through the shared CUTLASS bias tile |
