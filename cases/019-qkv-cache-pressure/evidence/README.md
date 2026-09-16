# 证据来源

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-qkv-matmul-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-qkv-matmul-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-qkv-matmul-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-019.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-019.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-019.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019.json](results/pi05-rtx5090/gpt6-run-01/measurements/019.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-019.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-019.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-019.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/019-triton-qkv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/019-triton-qkv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/019-triton-qkv-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/expert_qkv_matmul_screen.md](lab/pi05/expert_qkv_matmul_screen.md) | `lab/pi05/expert_qkv_matmul_screen.md` | git 21d95c3 的历史文件 |
| [patches/201d3b2.patch](patches/201d3b2.patch) | `git show 201d3b2 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；201d3b2063393b41a71c3b47961f39f634f6528b perf(pi05): add measured Triton expert QKV GEMM backend |
| [patches/0dcb021.patch](patches/0dcb021.patch) | `git show 0dcb021 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；0dcb021ddb7861b7e7cc7bfa89ceb80aa2774dfb Route expert QKV through the measured Triton GEMM |
