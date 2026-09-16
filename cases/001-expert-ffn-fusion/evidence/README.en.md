# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/profile-initial-summary.json](results/pi05-rtx5090/gpt6-run-01/profile-initial-summary.json) | `results/pi05-rtx5090/gpt6-run-01/profile-initial-summary.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/000.json](results/pi05-rtx5090/gpt6-run-01/measurements/000.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/000.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/001.json](results/pi05-rtx5090/gpt6-run-01/measurements/001.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/001.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/001-ffn-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/001-ffn-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/001-ffn-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/001-ffn-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/001-ffn-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/001-ffn-official.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/rtx5090_fused_ffn.md](lab/pi05/rtx5090_fused_ffn.md) | `lab/pi05/rtx5090_fused_ffn.md` | Historical file from git 21d95c3 |
| [patches/af667e0.patch](patches/af667e0.patch) | `git show af667e0 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; af667e0ecd360b78c24b655ffe0ff69e9a798ded Fuse Pi0.5 RTX 5090 expert FFN pointwise stages |
| [patches/23f3c8b.patch](patches/23f3c8b.patch) | `git show 23f3c8b -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 23f3c8b7457d701c13f740b9e5de1c79847e967b Route RTX 5090 Pi0.5 expert FFN through native fusion |
