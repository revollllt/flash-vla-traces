# 002 · Fuse Expert QKV pointwise stages, then validate the real model

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `c8f5e15`.

Use synthetic inputs for cheap screening; let full-model validation with real weights decide deployment.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

The initial Expert QKV chain accounted for about 7.768 ms and 4860 kernels across 180 calls. Normalization, casts and RoPE made the surrounding pointwise chain worth examining first.

## What evidence was available?

The existing BF16 GEMM could be retained. Pi0 and Pi0.5 arithmetic differed, so kernels from another Target were not directly reusable. The original rounding of RMS factor, input scaling, projection and final output had to remain.

## What was the hypothesis?

Use two Target-local CUDA kernels to fuse RMS/scale and factor/bias/RoPE/scatter, reducing launches and traversals without changing the GEMM. Start with synthetic inputs at the deployed shapes.

## What commands and changes followed?

Run `python -m lab.sm120.pi05_qkv_fusion_probe` with three seeds and input scales 1/0.001/1000; check Q/K/V, factor and KV-prefix sentinels. Integrate implementation 79a55c2, select its route in c8f5e15, then run official parity with the real checkpoint and deployment latency commands. Full arguments are in the trace and attached note.

## What were the results?

Synthetic checks were bitwise equal and prefixes unchanged. The two local Torch measurements were 53.7449/53.8509 µs versus 13.3714/13.3749 µs for the candidate. Real-weight official parity passed with action metrics identical to 001. Deployment median fell from 55.4515 to 49.8801 ms.

## Why retain, revert or investigate further?

Retain. The deployment control was 001, which already included FFN fusion, rather than the initial model; do not attribute both changes to QKV. Roughly 4× local synthetic speedup does not imply the same full-model speedup. This early deployment comparison was before/after, not ABBA.

## How to inspect the process

The excerpt contains 63 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 8, "function_call": 7, "function_call_output": 7}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [results/rtx5090-pi05/gpt6-qkv/README.md](evidence/results/rtx5090-pi05/gpt6-qkv/README.md)
