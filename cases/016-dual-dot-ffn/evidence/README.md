# 证据来源

源项目：`yx5090:/home/ubuntu/flash-vla`。下表是附带文件与原位置的映射，不依赖当前工作树中的后续修改。原说明中的绝对路径、相对链接、临时文件和命令参数保留历史语境；它们不保证在本仓库可执行。未复制权重、输入 safetensors、大型二进制与完整构建环境。

含凭据的文本已脱敏；JSON 保留原测量样本，不重算或选择性丢弃。补丁用于检查实际代码改动，不保证能直接应用到今天的主线。

| 本地文件 | 原始来源 | 来源说明 |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` 对应节 | 实验总结摘录；不代表事前已知 |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-dual-ffn-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-dual-ffn-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-dual-ffn-local.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015-control-before-016.json](results/pi05-rtx5090/gpt6-run-01/measurements/015-control-before-016.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015-control-before-016.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016.json](results/pi05-rtx5090/gpt6-run-01/measurements/016.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-repeat.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015-control-after-016.json](results/pi05-rtx5090/gpt6-run-01/measurements/015-control-after-016.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015-control-after-016.json` | 实验保存的结果文件（2026-09-16 读取） |
| [results/pi05-rtx5090/gpt6-run-01/correctness/016-dual-ffn-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/016-dual-ffn-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/016-dual-ffn-official.json` | 实验保存的结果文件（2026-09-16 读取） |
| [lab/pi05/expert_dual_dot_screen.md](lab/pi05/expert_dual_dot_screen.md) | `lab/pi05/expert_dual_dot_screen.md` | git 21d95c3 的历史文件 |
| [artifacts/rtx5090-pi05/gpt6-expert-dual-dot-screen-16x64x32.ptx](artifacts/rtx5090-pi05/gpt6-expert-dual-dot-screen-16x64x32.ptx) | `artifacts/rtx5090-pi05/gpt6-expert-dual-dot-screen-16x64x32.ptx` | 实验保存的结果文件（2026-09-16 读取） |
| [patches/6809f9f.patch](patches/6809f9f.patch) | `git show 6809f9f -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；6809f9fcc35f599476ea949a41eac9978b818378 Add rounded dual-dot backend for Pi05 expert FFN |
| [patches/78c8ca1.patch](patches/78c8ca1.patch) | `git show 78c8ca1 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | 历史实现/路由补丁；78c8ca1758f43cd4674fcd11108c91d67c31e634 Route expert FFN through rounded dual-dot fusion |
