# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-initial-failure.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-initial-failure.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-initial-failure.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011-control-before-012.json](results/pi05-rtx5090/gpt6-run-01/measurements/011-control-before-012.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011-control-before-012.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012.json](results/pi05-rtx5090/gpt6-run-01/measurements/012.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011-control-after-012.json](results/pi05-rtx5090/gpt6-run-01/measurements/011-control-after-012.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011-control-after-012.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/012-expert-epilogue-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/012-expert-epilogue-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/012-expert-epilogue-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/012-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/012-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/012-target-binding.txt` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/rtx5090_expert_epilogue.md](lab/pi05/rtx5090_expert_epilogue.md) | `lab/pi05/rtx5090_expert_epilogue.md` | git 21d95c3 的历史文件 |
| [artifacts/rtx5090-pi05/gpt6-epilogue-sanity.jsonl](artifacts/rtx5090-pi05/gpt6-epilogue-sanity.jsonl) | `artifacts/rtx5090-pi05/gpt6-epilogue-sanity.jsonl` | 实验保存的结果文件（2026-09-16 读取） |
| [artifacts/rtx5090-pi05/gpt6-epilogue-sanity-fixed.jsonl](artifacts/rtx5090-pi05/gpt6-epilogue-sanity-fixed.jsonl) | `artifacts/rtx5090-pi05/gpt6-epilogue-sanity-fixed.jsonl` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/94ef98a.patch](patches/94ef98a.patch) | `git show 94ef98a -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；94ef98abf071e300cd23d52bac291f5d2a97264e Add rounded CUTLASS epilogue for Pi0.5 expert down |
| [patches/df996b8.patch](patches/df996b8.patch) | `git show df996b8 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；df996b832f9a6de1250f0d6193544abeb1b5bc31 Deploy rounded expert down epilogue on RTX 5090 Pi0.5 |
