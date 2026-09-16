# 010 · Reuse normalization and fuse Prefix QKV RoPE/scatter

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `7085a4b`.

Reuse existing primitives within the Target, but validate the new composition on its actual call boundary.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Prefix QKV in the backbone still combined normalization, BF16 GEMM, casts, rotation and Q/K/V scatter, leaving surrounding fusion opportunities.

## What evidence was available?

The Target already had reusable RMSNorm. The source required rounding after normalization and again after projection. The actual 18-layer layout and call count were known; local gains from another call site could not simply be transferred.

## What was the hypothesis?

Retain the BF16 QKV GEMM and combine existing RMSNorm with one native cast/rotation/scatter pass to reduce intermediate traversals and launches.

## What commands and changes followed?

Implement the prefix QKV wrapper and run `python -m lab.pi05.rtx5090_prefix_qkv --seed 42`. Check Q/K/V and normalized values at actual layers 0/9/17, then time the 18-call graph. Select the deployment route in 7085a4b and run official parity and latency commands.

## What were the results?

Selected layers were bitwise equal. Local time fell from 137.960 to 58.0524 µs per call, an extrapolated 1.4383 ms over 18 calls. Official action cosine was 0.9999866853 and rel_rms 0.00516067. Deployment median fell from 35.3236 to 33.9966 ms. CPU declarations also passed without a compiler environment.

## Why retain, revert or investigate further?

Retain. The 1.3271 ms deployment difference exceeded within-run spread, with all 18 layers, 10 denoise steps and the BF16 workload preserved. Cumulative improvement is not evidence that optimization is exhausted; locate remaining hotspots with an updated profile.

## How to inspect the process

The excerpt contains 47 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 9, "function_call_output": 9, "agent_message": 6}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/rtx5090_prefix_qkv.md](evidence/lab/pi05/rtx5090_prefix_qkv.md)
