# 003 · Fuse Backbone FFN pointwise work while isolating per-layer numerical error

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `571b9b4`.

Capture the reference trajectory and restore reference outputs so upstream changes do not contaminate per-layer validation.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

The initial backbone FFN normalization, up-projection and activation chain took about 15.466 ms across 272 launches. Beyond the large GEMMs, repeated FP32 conversions and intermediate traversals remained.

## What evidence was available?

Source inspection allowed both large GEMMs to stay while replacing RMSNorm and GELU/product. The worker captured 17 calls from the actual reference trajectory, compared each candidate output, then restored the reference output before advancing to later layers.

## What was the hypothesis?

Fuse normalization by row and GELU/product into one final write, reducing full-tensor FP32 intermediates and repeated traversals. Keep the model precision policy and GEMM path unchanged.

## What commands and changes followed?

Implement fused_backbone in a separate worktree. Use the probe commands preserved in the trace for CUDA compilation, actual-call snapshots, per-layer comparison and graph timing. Integrate 0b062b3, select the FFN route in 571b9b4, and run full official parity and fresh-process deployment timing.

## What were the results?

All 17 layers passed existing tolerances; worst output rel_rms was 1.14e-4 and minimum cosine 0.9999999935. Local Torch totals were 15.698/15.975 ms versus 11.936/11.971 ms for the candidate. Full official parity passed, and deployment median fell from 49.8801 to 45.8682 ms.

## Why retain, revert or investigate further?

Retain. Roughly 0.28 ms local control drift was smaller than the candidate difference but must still be reported. Outputs were not all bitwise equal. Prefix-KV numerical changes remained within existing tolerances; this is neither unchanged output nor a change to the precision policy.

## How to inspect the process

The excerpt contains 59 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 11, "function_call": 8, "function_call_output": 8}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
