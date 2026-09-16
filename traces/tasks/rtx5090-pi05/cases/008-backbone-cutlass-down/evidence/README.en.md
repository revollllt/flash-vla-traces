# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/007.json](results/pi05-rtx5090/gpt6-run-01/measurements/007.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/007.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/008.json](results/pi05-rtx5090/gpt6-run-01/measurements/008.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/008.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/007-cutlass-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/007-cutlass-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/007-cutlass-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/008-cutlass-down-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/008-cutlass-down-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/008-cutlass-down-official.json` | Saved experimental result (read on 2026-09-16) |
| [patches/6a04c67.patch](patches/6a04c67.patch) | `git show 6a04c67 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 6a04c67db51b1582681c9f21be6cc06d900dc7fd Add Pi05 CUTLASS backbone GEMM candidate |
| [patches/54b5946.patch](patches/54b5946.patch) | `git show 54b5946 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 54b59465246eeb631eee006f810853a7f9e2baaa Deploy CUTLASS backbone down projection on RTX 5090 Pi0.5 |
