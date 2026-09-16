# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/004.json](results/pi05-rtx5090/gpt6-run-01/measurements/004.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/004.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/005.json](results/pi05-rtx5090/gpt6-run-01/measurements/005.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/005.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/005-attention-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/005-attention-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/005-attention-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention/README.md](results/rtx5090-pi05/gpt6-attention/README.md) | `results/rtx5090-pi05/gpt6-attention/README.md` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention/local.json](results/rtx5090-pi05/gpt6-attention/local.json) | `results/rtx5090-pi05/gpt6-attention/local.json` | Saved experimental result (read on 2026-09-16) |
| [patches/80cf420.patch](patches/80cf420.patch) | `git show 80cf420 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 80cf4201ae98da4ad95df46ebe963c9e06f34524 Fuse Pi0.5 expert attention score and softmax stages |
| [patches/4d53ad7.patch](patches/4d53ad7.patch) | `git show 4d53ad7 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 4d53ad73357ec8aa88e7b9ae9f6cded7a61bcdfd Deploy fused masked expert attention on RTX 5090 Pi0.5 |
