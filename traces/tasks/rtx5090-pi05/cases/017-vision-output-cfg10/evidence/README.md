# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-vision-outproj-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-vision-outproj-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-vision-outproj-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-control-before-017.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-control-before-017.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-control-before-017.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017.json](results/pi05-rtx5090/gpt6-run-01/measurements/017.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-control-after-017.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-control-after-017.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-control-after-017.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/017-vision-outproj-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/017-vision-outproj-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/017-vision-outproj-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/cutlass_vision_chain.md](lab/pi05/cutlass_vision_chain.md) | `lab/pi05/cutlass_vision_chain.md` | git 21d95c3 的历史文件 |
| [patches/d1b6f99.patch](patches/d1b6f99.patch) | `git show d1b6f99 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；d1b6f9996ea3139ba623138e26b992acd2dd18af Reuse vision cfg10 for attention output projection |
| [patches/eb8c16a.patch](patches/eb8c16a.patch) | `git show eb8c16a -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；eb8c16a18c70dbf0d760db358e19fc6ab4522d38 Route vision output projection through the shared CUTLASS tile |
