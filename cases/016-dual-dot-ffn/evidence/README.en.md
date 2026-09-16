# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-dual-ffn-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-dual-ffn-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-dual-ffn-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015-control-before-016.json](results/pi05-rtx5090/gpt6-run-01/measurements/015-control-before-016.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015-control-before-016.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016.json](results/pi05-rtx5090/gpt6-run-01/measurements/016.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015-control-after-016.json](results/pi05-rtx5090/gpt6-run-01/measurements/015-control-after-016.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015-control-after-016.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/016-dual-ffn-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/016-dual-ffn-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/016-dual-ffn-official.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/expert_dual_dot_screen.md](lab/pi05/expert_dual_dot_screen.md) | `lab/pi05/expert_dual_dot_screen.md` | Historical file from git 21d95c3 |
| [artifacts/rtx5090-pi05/gpt6-expert-dual-dot-screen-16x64x32.ptx](artifacts/rtx5090-pi05/gpt6-expert-dual-dot-screen-16x64x32.ptx) | `artifacts/rtx5090-pi05/gpt6-expert-dual-dot-screen-16x64x32.ptx` | Saved experimental result (read on 2026-09-16) |
| [patches/6809f9f.patch](patches/6809f9f.patch) | `git show 6809f9f -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 6809f9fcc35f599476ea949a41eac9978b818378 Add rounded dual-dot backend for Pi05 expert FFN |
| [patches/78c8ca1.patch](patches/78c8ca1.patch) | `git show 78c8ca1 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 78c8ca1758f43cd4674fcd11108c91d67c31e634 Route expert FFN through rounded dual-dot fusion |
