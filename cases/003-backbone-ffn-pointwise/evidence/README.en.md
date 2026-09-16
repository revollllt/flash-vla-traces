# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/002.json](results/pi05-rtx5090/gpt6-run-01/measurements/002.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/002.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/003.json](results/pi05-rtx5090/gpt6-run-01/measurements/003.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/003.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/003-backbone-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/003-backbone-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/003-backbone-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/003-backbone-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/003-backbone-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/003-backbone-official.json` | Saved experimental result (read on 2026-09-16) |
| [patches/0b062b3.patch](patches/0b062b3.patch) | `git show 0b062b3 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 0b062b37f286a3469ca977b100692963fac6a0a1 Fuse Pi05 RTX5090 backbone FFN pointwise stages |
| [patches/571b9b4.patch](patches/571b9b4.patch) | `git show 571b9b4 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 571b9b4fc5177dac96a0d8404b14195ef8ddcff6 Route RTX 5090 Pi0.5 backbone FFN through native fusion |
