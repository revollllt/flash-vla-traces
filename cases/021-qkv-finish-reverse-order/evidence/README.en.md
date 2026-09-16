# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-local-qkv-finish.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-local-qkv-finish.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-local-qkv-finish.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020-control-before-021.json](results/pi05-rtx5090/gpt6-run-01/measurements/020-control-before-021.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020-control-before-021.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021.json](results/pi05-rtx5090/gpt6-run-01/measurements/021.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020-control-after-021.json](results/pi05-rtx5090/gpt6-run-01/measurements/020-control-after-021.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020-control-after-021.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a1.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a1.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a2.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-a2.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b1.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b1.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b2.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-reverse-b2.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/021-decision.json](results/pi05-rtx5090/gpt6-run-01/measurements/021-decision.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/021-decision.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/021-qkv-finish-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/021-qkv-finish-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/021-qkv-finish-official.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/expert_qkv_finish_screen.md](lab/pi05/expert_qkv_finish_screen.md) | `lab/pi05/expert_qkv_finish_screen.md` | Historical file from git 21d95c3 |
| [patches/c070e92.patch](patches/c070e92.patch) | `git show c070e92 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; c070e924829e1e7a6d18088cf8006d517d7e8b33 perf(pi05): add rounded QKV GEMM epilogue fusion backend |
| [patches/4de57c9.patch](patches/4de57c9.patch) | `git show 4de57c9 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 4de57c91b0ce1e22ad8350836f2ca3f292e1e90e Route expert QKV through the rounded GEMM epilogue fusion |
