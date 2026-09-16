# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010-control-before-011.json](results/pi05-rtx5090/gpt6-run-01/measurements/010-control-before-011.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010-control-before-011.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011.json](results/pi05-rtx5090/gpt6-run-01/measurements/011.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/011-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010-control-after-011.json](results/pi05-rtx5090/gpt6-run-01/measurements/010-control-after-011.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010-control-after-011.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/011-action-out-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/011-action-out-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/011-action-out-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/011-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/011-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/011-target-binding.txt` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-action-out/README.md](results/rtx5090-pi05/gpt6-action-out/README.md) | `results/rtx5090-pi05/gpt6-action-out/README.md` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-action-out/local.json](results/rtx5090-pi05/gpt6-action-out/local.json) | `results/rtx5090-pi05/gpt6-action-out/local.json` | Saved experimental result (read on 2026-09-16) |
| [patches/fc73104.patch](patches/fc73104.patch) | `git show fc73104 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; fc731043760d885117b14b882e9719bc3d19cf3b Fuse Pi0.5 action output normalization and Euler update |
| [patches/853d1fa.patch](patches/853d1fa.patch) | `git show 853d1fa -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 853d1fa9e12ef620fde30b774f21f32103fc28e9 Deploy fused action output pointwise stages on RTX 5090 Pi0.5 |
