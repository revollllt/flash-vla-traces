# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-qkv-matmul-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-qkv-matmul-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-qkv-matmul-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-019.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-019.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-019.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019.json](results/pi05-rtx5090/gpt6-run-01/measurements/019.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-019.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-019.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-019.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/019-triton-qkv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/019-triton-qkv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/019-triton-qkv-official.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/expert_qkv_matmul_screen.md](lab/pi05/expert_qkv_matmul_screen.md) | `lab/pi05/expert_qkv_matmul_screen.md` | Historical file from git 21d95c3 |
| [patches/201d3b2.patch](patches/201d3b2.patch) | `git show 201d3b2 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 201d3b2063393b41a71c3b47961f39f634f6528b perf(pi05): add measured Triton expert QKV GEMM backend |
| [patches/0dcb021.patch](patches/0dcb021.patch) | `git show 0dcb021 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 0dcb021ddb7861b7e7cc7bfa89ceb80aa2774dfb Route expert QKV through the measured Triton GEMM |
