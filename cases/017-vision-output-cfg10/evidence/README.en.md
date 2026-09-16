# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-vision-outproj-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-vision-outproj-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-vision-outproj-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-control-before-017.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-control-before-017.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-control-before-017.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017.json](results/pi05-rtx5090/gpt6-run-01/measurements/017.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/017-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/017-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/017-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/016-control-after-017.json](results/pi05-rtx5090/gpt6-run-01/measurements/016-control-after-017.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/016-control-after-017.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/017-vision-outproj-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/017-vision-outproj-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/017-vision-outproj-official.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/cutlass_vision_chain.md](lab/pi05/cutlass_vision_chain.md) | `lab/pi05/cutlass_vision_chain.md` | Historical file from git 21d95c3 |
| [patches/d1b6f99.patch](patches/d1b6f99.patch) | `git show d1b6f99 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; d1b6f9996ea3139ba623138e26b992acd2dd18af Reuse vision cfg10 for attention output projection |
| [patches/eb8c16a.patch](patches/eb8c16a.patch) | `git show eb8c16a -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; eb8c16a18c70dbf0d760db358e19fc6ab4522d38 Route vision output projection through the shared CUTLASS tile |
