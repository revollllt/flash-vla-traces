# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-outproj-epilogue-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-outproj-epilogue-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-outproj-epilogue-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-control-before-014.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-control-before-014.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-control-before-014.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014.json](results/pi05-rtx5090/gpt6-run-01/measurements/014.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-control-after-014.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-control-after-014.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-control-after-014.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/014-outproj-epilogue-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/014-outproj-epilogue-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/014-outproj-epilogue-official.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/rtx5090_expert_epilogue.md](lab/pi05/rtx5090_expert_epilogue.md) | `lab/pi05/rtx5090_expert_epilogue.md` | Historical file from git 21d95c3 |
| [patches/9de2c9c.patch](patches/9de2c9c.patch) | `git show 9de2c9c -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 9de2c9cd72ddbfd596016939e6a68cbe0a97ee62 Reuse Pi0.5 gated epilogue for expert attention output |
| [patches/7669007.patch](patches/7669007.patch) | `git show 7669007 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 76690072d4c526bdabb807aa3434398a95a1477a Deploy shared rounded epilogue for expert output projection |
