# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-018.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-018.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-before-018.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018.json](results/pi05-rtx5090/gpt6-run-01/measurements/018.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-018.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-018.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-control-after-018.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a1.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a1.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a2.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-a2.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b1.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b1.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b2.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-baab-b2.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/018-decision.json](results/pi05-rtx5090/gpt6-run-01/measurements/018-decision.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/018-decision.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/018-triton-pv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/018-triton-pv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/018-triton-pv-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/README.md](results/rtx5090-pi05/gpt6-attention-pv-triton/README.md) | `results/rtx5090-pi05/gpt6-attention-pv-triton/README.md` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/dispatch.json](results/rtx5090-pi05/gpt6-attention-pv-triton/dispatch.json) | `results/rtx5090-pi05/gpt6-attention-pv-triton/dispatch.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/local.json](results/rtx5090-pi05/gpt6-attention-pv-triton/local.json) | `results/rtx5090-pi05/gpt6-attention-pv-triton/local.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-pv-triton/representative.json](results/rtx5090-pi05/gpt6-attention-pv-triton/representative.json) | `results/rtx5090-pi05/gpt6-attention-pv-triton/representative.json` | Saved experimental result (read on 2026-09-16) |
| [patches/7b1f6df.patch](patches/7b1f6df.patch) | `git show 7b1f6df -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 7b1f6dfb760333a5c350505b427c16eb300ff89e Add optional Pi0.5 attention backend with one PV launch |
| [patches/34add1b.patch](patches/34add1b.patch) | `git show 34add1b -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 34add1bc19f1b8bee46e10689e2b8a0af5363c7d Route expert attention through the single-launch PV backend |
| [patches/1542c05.patch](patches/1542c05.patch) | `git show 1542c05 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 1542c056405270475aff96bc9b7a3121f8058e18 Withdraw PV route after deployment timing remained sensitive to drift |
