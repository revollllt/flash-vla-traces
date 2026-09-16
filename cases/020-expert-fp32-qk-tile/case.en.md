# 020 · Replace only Expert QK with a fixed FP32-score Triton tile

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `76086d9`.

Launch count is not the only target; retain the downstream chain to isolate a GEMM implementation change.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

QK already used one launch per call. Attribution in 010 totaled about 0.804 ms across 180 calls, without a separate copy or reduction to eliminate.

## What evidence was available?

Q(400,256) and K(1018,256) are BF16 and produce FP32 logits; the transpose is a view. Existing softmax, BF16 probabilities and PV can remain. With out=Q, capture and each complete-chain timing invocation require restoring Q.

## What was the hypothesis?

A fixed tile and loading path might improve QK scheduling and reuse. Compare only three tiles, without padding, packing, softmax fusion or autotuning.

## What commands and changes followed?

Run the QK probe documented in the attached note. Screen 32×32×64, 32×64×64 and 32×64×128 on one real input, then check the winner on nine real step/layer cases and the full attention chain with equal Q reset. Deploy only 32×32×64 while retaining softmax/PV. After official validation, run end-to-end ABBA.

## What were the results?

All nine logits, probability and final-output comparisons were bitwise equal. The local nine-call chain changed from A118.314/118.315 to B110.126/110.133 µs. Official action metrics matched 019. Deployment A32.406389/32.362382 became B32.171719/32.211052 ms, a 0.193000 ms mean-of-medians improvement.

## Why retain, revert or investigate further?

Retain: every candidate median was below every control, with A/B drift 0.044007/0.039333 ms. One sample at each matching endpoint clock on both routes does not establish matching frequency histories. The full deployment gain cannot be assigned entirely to the isolated QK timing difference.

## How to inspect the process

The excerpt contains 96 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 16, "function_call_output": 16, "agent_message": 18}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [results/rtx5090-pi05/gpt6-attention-qk-triton/README.md](evidence/results/rtx5090-pi05/gpt6-attention-qk-triton/README.md)
