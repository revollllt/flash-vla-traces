# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-control-before-015.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-control-before-015.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-control-before-015.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015.json](results/pi05-rtx5090/gpt6-run-01/measurements/015.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/015-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/015-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/015-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/014-control-after-015.json](results/pi05-rtx5090/gpt6-run-01/measurements/014-control-after-015.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/014-control-after-015.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/015-vision-qkv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/015-vision-qkv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/015-vision-qkv-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md](results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md) | `results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-vision-qkv-cfg10/local.json](results/rtx5090-pi05/gpt6-vision-qkv-cfg10/local.json) | `results/rtx5090-pi05/gpt6-vision-qkv-cfg10/local.json` | Saved experimental result (read on 2026-09-16) |
| [patches/9215399.patch](patches/9215399.patch) | `git show 9215399 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 9215399dae787fea819d95d1f89c0a850bf0e429 Reuse the vision cfg10 bias GEMM for QKV projection |
| [patches/5772cf2.patch](patches/5772cf2.patch) | `git show 5772cf2 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 5772cf204fe8ffc752e6463eb2eca21a5768e79d Route vision QKV through the shared CUTLASS bias tile |
