# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/008.json](results/pi05-rtx5090/gpt6-run-01/measurements/008.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/008.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/009.json](results/pi05-rtx5090/gpt6-run-01/measurements/009.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/009.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/009-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/009-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/009-target-binding.txt` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/009-vision-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/009-vision-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/009-vision-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-vision/README.md](results/rtx5090-pi05/gpt6-vision/README.md) | `results/rtx5090-pi05/gpt6-vision/README.md` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-vision/local.json](results/rtx5090-pi05/gpt6-vision/local.json) | `results/rtx5090-pi05/gpt6-vision/local.json` | Saved experimental result (read on 2026-09-16) |
| [patches/a6da2bb.patch](patches/a6da2bb.patch) | `git show a6da2bb -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; a6da2bbbc64818e616ddcc3c3b6c868044987fc1 Fuse Pi0.5 vision normalization and activation stages |
| [patches/cc5f0a8.patch](patches/cc5f0a8.patch) | `git show cc5f0a8 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; cc5f0a8db9292b8e1f216a3973408dc2470258d7 Deploy fused vision normalization and activation on RTX 5090 Pi0.5 |
