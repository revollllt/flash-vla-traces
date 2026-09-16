# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/009.json](results/pi05-rtx5090/gpt6-run-01/measurements/009.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/009.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010.json](results/pi05-rtx5090/gpt6-run-01/measurements/010.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/010-prefix-qkv-local.json](results/pi05-rtx5090/gpt6-run-01/measurements/010-prefix-qkv-local.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/010-prefix-qkv-local.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/010-prefix-qkv-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/010-prefix-qkv-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/010-prefix-qkv-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/010-target-binding.txt](results/pi05-rtx5090/gpt6-run-01/correctness/010-target-binding.txt) | `results/pi05-rtx5090/gpt6-run-01/correctness/010-target-binding.txt` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/rtx5090_prefix_qkv.md](lab/pi05/rtx5090_prefix_qkv.md) | `lab/pi05/rtx5090_prefix_qkv.md` | Historical file from git 21d95c3 |
| [patches/221de0c.patch](patches/221de0c.patch) | `git show 221de0c -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 221de0c711d8eb41b77313a4fa4a92c23df083e3 Fuse Pi0.5 RTX 5090 backbone QKV pointwise stages |
| [patches/7085a4b.patch](patches/7085a4b.patch) | `git show 7085a4b -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 7085a4ba9f9c3e3e31ce6cbe0f6db853f56ec6d3 Deploy fused prefix QKV on RTX 5090 Pi0.5 |
