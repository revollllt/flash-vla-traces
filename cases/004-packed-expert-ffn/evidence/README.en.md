# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/003.json](results/pi05-rtx5090/gpt6-run-01/measurements/003.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/003.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/004.json](results/pi05-rtx5090/gpt6-run-01/measurements/004.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/004.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/004-packed-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/004-packed-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/004-packed-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/004-packed-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/004-packed-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/004-packed-official.json` | Saved experimental result (read on 2026-09-16) |
| [patches/622f161.patch](patches/622f161.patch) | `git show 622f161 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 622f1611954e3e38ca7878d6eafc48d52e6a82f5 Pack Pi0.5 RTX 5090 expert gate and up into one GEMM |
| [patches/ebfde75.patch](patches/ebfde75.patch) | `git show ebfde75 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; ebfde7594551bde089d81da3954e57a4dc61e18a Deploy packed expert FFN on RTX 5090 Pi0.5 |
