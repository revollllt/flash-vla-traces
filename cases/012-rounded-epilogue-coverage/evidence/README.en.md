# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-initial-failure.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-initial-failure.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-initial-failure.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-expert-epilogue-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011-control-before-012.json](results/pi05-rtx5090/gpt6-run-01/measurements/011-control-before-012.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011-control-before-012.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012.json](results/pi05-rtx5090/gpt6-run-01/measurements/012.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/011-control-after-012.json](results/pi05-rtx5090/gpt6-run-01/measurements/011-control-after-012.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/011-control-after-012.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/012-expert-epilogue-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/012-expert-epilogue-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/012-expert-epilogue-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/012-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/012-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/012-target-binding.txt` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/rtx5090_expert_epilogue.md](lab/pi05/rtx5090_expert_epilogue.md) | `lab/pi05/rtx5090_expert_epilogue.md` | Historical file from git 21d95c3 |
| [artifacts/rtx5090-pi05/gpt6-epilogue-sanity.jsonl](artifacts/rtx5090-pi05/gpt6-epilogue-sanity.jsonl) | `artifacts/rtx5090-pi05/gpt6-epilogue-sanity.jsonl` | Saved experimental result (read on 2026-09-16) |
| [artifacts/rtx5090-pi05/gpt6-epilogue-sanity-fixed.jsonl](artifacts/rtx5090-pi05/gpt6-epilogue-sanity-fixed.jsonl) | `artifacts/rtx5090-pi05/gpt6-epilogue-sanity-fixed.jsonl` | Saved experimental result (read on 2026-09-16) |
| [patches/94ef98a.patch](patches/94ef98a.patch) | `git show 94ef98a -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 94ef98abf071e300cd23d52bac291f5d2a97264e Add rounded CUTLASS epilogue for Pi0.5 expert down |
| [patches/df996b8.patch](patches/df996b8.patch) | `git show df996b8 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; df996b832f9a6de1250f0d6193544abeb1b5bc31 Deploy rounded expert down epilogue on RTX 5090 Pi0.5 |
