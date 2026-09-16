# 017 · Reuse cfg10 for Vision output projection while retaining the separate BF16 residual add

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `eb8c16a`.

Reuse and fusion are different hypotheses; preserve rounding order and independently test the new shape.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Vision output projection still offered local GEMM/residual-chain headroom. Deployed cfg10 could serve the smaller M768/K1152/N1152 shape.

## What evidence was available?

The main difference from Vision FFN down is K, which the existing residual wrapper can read from the weight shape. Semantics require accumulator+bias to round to BF16 before the separate residual add. The site has 27 layers and a 68.34375 MiB weight set that can fit in L2.

## What was the hypothesis?

Reuse cfg10 and the wrapper without adding a native kernel or ABI, seeking lower GEMM time. Do not fuse the residual in this trial; preserve the two rounding steps.

## What commands and changes followed?

Select the Vision output site in `lab.pi05.cutlass_vision_chain`, check all 27 layer outputs and repeated replay, then run full-chain ABBA. Integrate d1b6f99 and select only this route in eb8c16a. Run official parity and four fresh deployment processes.

## What were the results?

All 27 layers passed existing tolerances, with worst rel_rms 0.00105418. Outputs were not bitwise equal to cuBLAS, although candidate replays were identical. Local ABBA was A0.587776/B0.454656/B0.454656/A0.587776 ms. Deployment A32.771943/32.782361 became B32.610786/32.613452 ms, improving mean medians by 0.165034 ms.

## Why retain, revert or investigate further?

Official parity passed and deployment A/B drift was 0.010418/0.002666 ms, so retain. Measure deployment independently because the local weights can reside in L2. Passing tolerance and identical repeated candidate replay must not be described as bitwise equality to the reference.

## How to inspect the process

The excerpt contains 48 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 8, "function_call": 7, "function_call_output": 6}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/cutlass_vision_chain.md](evidence/lab/pi05/cutlass_vision_chain.md)
