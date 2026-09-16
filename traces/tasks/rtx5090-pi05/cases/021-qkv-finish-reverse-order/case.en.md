# 021 · Confirm a small QKV finish-fusion gain with reverse-order measurements

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `4de57c9`.

The same follow-up rule can confirm a small gain; fix the measurement order before seeing its results.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

After optimizing the QKV GEMM, factor/bias/RoPE/scatter still required a separate finish launch and projected buffer.

## What evidence was available?

Source inspection showed that adjacent RoPE column pairs fit within the existing 32-column tile and Q/K/V boundaries aligned with it. The caller already supplied K/V suffix views, so adding another prefix offset would write the wrong addresses. Moving finish arithmetic into the epilogue does not eliminate that arithmetic.

## What was the hypothesis?

Preserve an explicit BF16 roundtrip of the accumulator, then perform postprocessing and scatter inside the existing tile. This might remove the 256,000-byte scratch and one launch; compilation must establish the resource cost.

## What commands and changes followed?

Run `lab.pi05.expert_qkv_finish_screen`, checking Q/K/V, factor and prefix contents over 18 layers, plus PTX and resources. Integrate the sole candidate, run official parity and deployment ABBA. Because first-block candidate drift was close to the gain, predefine exactly one BAAB follow-up and judge all eight results.

## What were the results?

Local outputs and prefix contents were bitwise equal; resources remained 40 registers, 6 KiB shared memory and no spills. The local chain changed from A10.807111/10.775111 to B9.637333/9.630222 µs. Initial deployment mean improvement was 0.122573 ms with B drift of 0.100344 ms. The reverse block improved by 0.098972 ms, with A/B drift 0.002277/0.023303 ms and minimum separation 0.086182 ms.

## Why retain, revert or investigate further?

Retain: candidate and control medians stayed separated in both orders. The all-eight mean-of-medians difference was 0.110772 ms. The first candidate point plotted on the curve, 32.041923 ms, is an individual measurement, not the standalone incremental gain estimate. Read alongside case 018 to see the same follow-up protocol lead to a different decision.

## How to inspect the process

The excerpt contains 110 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 8, "function_call": 9, "function_call_output": 9}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/expert_qkv_finish_screen.md](evidence/lab/pi05/expert_qkv_finish_screen.md)
