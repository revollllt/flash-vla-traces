# 证据来源

[中文](README.md) | [English](README.en.md)

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/prefix-static-m896.json](results/pi05-rtx5090/gpt6-run-01/measurements/prefix-static-m896.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/prefix-static-m896.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a1.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a1.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a2.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a2.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-decision.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-decision.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-decision.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-plan-selection.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-plan-selection.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-plan-selection.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-protocol.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-protocol.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-protocol.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-resources.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-resources.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-resources.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023.json](results/pi05-rtx5090/gpt6-run-01/measurements/023.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-bucket-switch-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-bucket-switch-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-bucket-switch-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-dense-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-dense-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-dense-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-fixture-inputs.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-fixture-inputs.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-fixture-inputs.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-oracle-provenance.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-oracle-provenance.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-oracle-provenance.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/profile-023-summary.json](results/pi05-rtx5090/gpt6-run-01/profile-023-summary.json) | `results/pi05-rtx5090/gpt6-run-01/profile-023-summary.json` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/padding_runtime_rows.md](lab/pi05/padding_runtime_rows.md) | `lab/pi05/padding_runtime_rows.md` | git 21d95c3 的历史文件 |
| [lab/pi05/runtime_prefix_static.md](lab/pi05/runtime_prefix_static.md) | `lab/pi05/runtime_prefix_static.md` | git 21d95c3 的历史文件 |
| [lab/pi05/runtime_prefix_native_feasibility.md](lab/pi05/runtime_prefix_native_feasibility.md) | `lab/pi05/runtime_prefix_native_feasibility.md` | git 21d95c3 的历史文件 |
| [lab/pi05/bucket_switch.md](lab/pi05/bucket_switch.md) | `lab/pi05/bucket_switch.md` | git 21d95c3 的历史文件 |
| [patches/a1e20bd.patch](patches/a1e20bd.patch) | `git show a1e20bd -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；a1e20bd4b89bd6176ce91872e0117aadefa39935 Add device-masked static backbone GEMM bucket entries |
| [patches/7204975.patch](patches/7204975.patch) | `git show 7204975 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；7204975acf63d657039d332a26d737eb583d7828 Add explicit-mask backbone FFN bucket candidate |
| [patches/7869a5a.patch](patches/7869a5a.patch) | `git show 7869a5a -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；7869a5a4244bdd82036727308081d03539f0469b Select runtime-mask backbone FFN buckets on RTX 5090 |
