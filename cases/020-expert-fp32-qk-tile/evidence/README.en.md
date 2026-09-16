# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-control-before-020.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-control-before-020.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-control-before-020.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020.json](results/pi05-rtx5090/gpt6-run-01/measurements/020.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/020-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/020-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/020-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/019-control-after-020.json](results/pi05-rtx5090/gpt6-run-01/measurements/019-control-after-020.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/019-control-after-020.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/020-triton-qk-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/020-triton-qk-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/020-triton-qk-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/README.md](results/rtx5090-pi05/gpt6-attention-qk-triton/README.md) | `results/rtx5090-pi05/gpt6-attention-qk-triton/README.md` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/local.json](results/rtx5090-pi05/gpt6-attention-qk-triton/local.json) | `results/rtx5090-pi05/gpt6-attention-qk-triton/local.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/representative.json](results/rtx5090-pi05/gpt6-attention-qk-triton/representative.json) | `results/rtx5090-pi05/gpt6-attention-qk-triton/representative.json` | Saved experimental result (read on 2026-09-16) |
| [results/rtx5090-pi05/gpt6-attention-qk-triton/dispatch.json](results/rtx5090-pi05/gpt6-attention-qk-triton/dispatch.json) | `results/rtx5090-pi05/gpt6-attention-qk-triton/dispatch.json` | Saved experimental result (read on 2026-09-16) |
| [patches/0c20f24.patch](patches/0c20f24.patch) | `git show 0c20f24 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 0c20f245be406c3e40f33c6b343b65272e5dd388 Add optional Pi0.5 attention backend with fixed FP32 QK |
| [patches/76086d9.patch](patches/76086d9.patch) | `git show 76086d9 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 76086d9564d4083860cf3335843742e3ad9f5b87 Route expert QK through the measured FP32-score tile |
