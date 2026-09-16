# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/005.json](results/pi05-rtx5090/gpt6-run-01/measurements/005.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/005.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/006.json](results/pi05-rtx5090/gpt6-run-01/measurements/006.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/006.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/006-residual-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/006-residual-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/006-residual-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/006-residual-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/006-residual-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/006-residual-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/006-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/006-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/006-target-binding.txt` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/rtx5090_fused_residual.md](lab/pi05/rtx5090_fused_residual.md) | `lab/pi05/rtx5090_fused_residual.md` | Historical file from git 21d95c3 |
| [patches/da1ebf5.patch](patches/da1ebf5.patch) | `git show da1ebf5 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; da1ebf57cbe36bb2105515b134dba7930a7957a5 Fuse Pi0.5 RTX 5090 expert gated residual updates |
| [patches/e98b74c.patch](patches/e98b74c.patch) | `git show e98b74c -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; e98b74c0711ea75fe658e01101c8a27a1e27b2af Deploy fused gated residuals on RTX 5090 Pi0.5 |
| [patches/e9875fa.patch](patches/e9875fa.patch) | `git show e9875fa -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; e9875fa1be420cdb4464c3d4c4e0ead4e73ef381 Keep native backbone loading lazy and record residual deployment gain |
