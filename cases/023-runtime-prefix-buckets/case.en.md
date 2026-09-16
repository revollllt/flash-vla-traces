# 023 · Find padding headroom in real masks, then validate runtime FFN buckets incrementally

[中文](case.md) | [English](case.en.md)

Outcome: **Conditionally retained**. Historical code reference: `7869a5a`.

Screen a cheap static upper bound before paying for dynamic implementation; correctness inputs must exercise the optimized branch.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Backbone buffers physically held 968 rows, but effective language-prefix length varied by task. Some tail GEMM work might be unnecessary.

## What evidence was available?

Two actual seed42 recordings both had 127 language tokens and 895 valid prefix rows; the existing seed0 official fixture had 903 rows. A static hypothesis must not hardcode the seed42 length. Source inspection also found host-initialized GEMM parameters, requiring attention to runtime selection, workspaces and stale tail values.

## What was the hypothesis?

First compare M896 and M968 with existing cfg0 to establish whether saving can cover a dynamic implementation. If promising, use the device mask to select between two static plans. This selection relies on the project’s contiguous valid-prefix layout and does not apply to arbitrary sparse masks.

## What commands and changes followed?

Run `lab.pi05.runtime_prefix_static`, then implement Target-local explicit-mask operators, separate workspaces and two entries in one graph. The short path zeros the old gate/up tail before reading it, while external KV layout and suffix offset stay at 968. Run `lab.pi05.bucket_switch` with new short and existing long fixtures as short896→long903→short896 in the same engine/graph. Finally run four consecutive fresh processes in ABBA order.

## What were the results?

The static 51-GEMM screen improved mean medians by 1.673216 ms with 0.290816 ms control drift; this only justified implementation. All three dynamic official comparisons passed. Deployment was A32.061675/B30.315662/B30.315381/A32.147649 ms, improving by 1.789140 ms, well beyond A/B drift of 0.085974/0.000281 ms. Workspace grew from about 22.3 MB to 44.6 MB.

## Why retain, revert or investigate further?

Retain conditionally: the performance claim covers the measured valid_prefix≤896 input. Long-prefix numerics passed, but a latency benefit was not established. The original official fixture did not exercise the short branch, so a boundary input was necessary. The oracle used a locally modified vendored OpenPI and compatible dependencies; this is not pristine upstream validation or robot task-success evidence.

## How to inspect the process

The excerpt contains 329 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 71, "function_call": 62, "function_call_output": 61}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/padding_runtime_rows.md](evidence/lab/pi05/padding_runtime_rows.md)
- [lab/pi05/runtime_prefix_static.md](evidence/lab/pi05/runtime_prefix_static.md)
- [lab/pi05/runtime_prefix_native_feasibility.md](evidence/lab/pi05/runtime_prefix_native_feasibility.md)
- [lab/pi05/bucket_switch.md](evidence/lab/pi05/bucket_switch.md)
