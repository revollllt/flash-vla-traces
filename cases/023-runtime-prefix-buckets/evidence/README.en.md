# Evidence provenance

[中文](README.md) | [English](README.en.md)

Source project: `yx5090:/home/ubuntu/flash-vla`. The table maps attached files to their original locations rather than to subsequent edits in the current source worktree. Historical absolute paths, relative links, temporary files and command arguments remain as provenance; they are not guaranteed to run from this repository. Weights, input safetensors, large binaries and the complete build environment are not included.

Credential-bearing text is redacted. JSON retains the original measurement samples without recalculation or selective removal. Patches document actual code changes and are not guaranteed to apply to today’s main branch. The Chinese and English guides share these original-language evidence files.

| Local file | Original source | Provenance |
|---|---|---|
| [run-note.md](run-note.md) | `results/pi05-rtx5090/gpt6-run-01/README.md` — corresponding section | Experiment-summary excerpt; not evidence of prior knowledge |
| [results/pi05-rtx5090/gpt6-run-01/measurements/prefix-static-m896.json](results/pi05-rtx5090/gpt6-run-01/measurements/prefix-static-m896.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/prefix-static-m896.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a1.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a1.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a1.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a2.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a2.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-control-a2.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-decision.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-decision.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-decision.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-plan-selection.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-plan-selection.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-plan-selection.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-protocol.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-protocol.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-protocol.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-repeat.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-repeat.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-repeat.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023-resources.json](results/pi05-rtx5090/gpt6-run-01/measurements/023-resources.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023-resources.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/measurements/023.json](results/pi05-rtx5090/gpt6-run-01/measurements/023.json) | `results/pi05-rtx5090/gpt6-run-01/measurements/023.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-bucket-switch-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-bucket-switch-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-bucket-switch-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-dense-official.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-dense-official.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-dense-official.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-fixture-inputs.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-fixture-inputs.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-fixture-inputs.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-oracle-provenance.json](results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-oracle-provenance.json) | `results/pi05-rtx5090/gpt6-run-01/correctness/023-short896-oracle-provenance.json` | Saved experimental result (read on 2026-09-16) |
| [results/pi05-rtx5090/gpt6-run-01/profile-023-summary.json](results/pi05-rtx5090/gpt6-run-01/profile-023-summary.json) | `results/pi05-rtx5090/gpt6-run-01/profile-023-summary.json` | Saved experimental result (read on 2026-09-16) |
| [lab/pi05/padding_runtime_rows.md](lab/pi05/padding_runtime_rows.md) | `lab/pi05/padding_runtime_rows.md` | Historical file from git 21d95c3 |
| [lab/pi05/runtime_prefix_static.md](lab/pi05/runtime_prefix_static.md) | `lab/pi05/runtime_prefix_static.md` | Historical file from git 21d95c3 |
| [lab/pi05/runtime_prefix_native_feasibility.md](lab/pi05/runtime_prefix_native_feasibility.md) | `lab/pi05/runtime_prefix_native_feasibility.md` | Historical file from git 21d95c3 |
| [lab/pi05/bucket_switch.md](lab/pi05/bucket_switch.md) | `lab/pi05/bucket_switch.md` | Historical file from git 21d95c3 |
| [patches/a1e20bd.patch](patches/a1e20bd.patch) | `git show a1e20bd -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; a1e20bd4b89bd6176ce91872e0117aadefa39935 Add device-masked static backbone GEMM bucket entries |
| [patches/7204975.patch](patches/7204975.patch) | `git show 7204975 -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 7204975acf63d657039d332a26d737eb583d7828 Add explicit-mask backbone FFN bucket candidate |
| [patches/7869a5a.patch](patches/7869a5a.patch) | `git show 7869a5a -- src/flash_vla/hardware/nvidia/rtx5090/pi05 lab/pi05/backbone_fullrow.py` | Historical implementation/routing patch; 7869a5a4244bdd82036727308081d03539f0469b Select runtime-mask backbone FFN buckets on RTX 5090 |
