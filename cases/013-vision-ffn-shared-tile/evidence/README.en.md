# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/vision-cutlass-screen.json](results/pi05-rtx5090/gpt6-run-01/measurements/vision-cutlass-screen.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/vision-cutlass-screen.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-vision-chain-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-vision-chain-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-vision-chain-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-control-before-013.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-control-before-013.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-control-before-013.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013.json](results/pi05-rtx5090/gpt6-run-01/measurements/013.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/013-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/013-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/013-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/012-control-after-013.json](results/pi05-rtx5090/gpt6-run-01/measurements/012-control-after-013.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/012-control-after-013.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/013-vision-cutlass-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/013-vision-cutlass-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/013-vision-cutlass-official.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/cutlass_vision_screen.md](lab/pi05/cutlass_vision_screen.md) | `lab/pi05/cutlass_vision_screen.md` | Historical file from git 21d95c3 |
| [lab/pi05/cutlass_vision_chain.md](lab/pi05/cutlass_vision_chain.md) | `lab/pi05/cutlass_vision_chain.md` | Historical file from git 21d95c3 |
| [patches/f22140f.patch](patches/f22140f.patch) | `git show f22140f -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; f22140fa759da074c34d5e4727525c6a2d24a4a9 Add cfg10 bias GEMM for Pi05 vision FFN chains |
| [patches/5f7ccf0.patch](patches/5f7ccf0.patch) | `git show 5f7ccf0 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 5f7ccf03a4f10853d719f01ec24946fc13357e78 Deploy common CUTLASS vision FFN tile on RTX 5090 Pi0.5 |
