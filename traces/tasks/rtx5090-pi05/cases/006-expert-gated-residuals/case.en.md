# 006 · Reuse a fused gated residual at two Expert projection sites

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `e98b74c`.

In-place residuals require equal resets; record declaration-time loading fixes separately from CUDA performance changes.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Expert attention output projection and FFN down each had seven cast/mul/add/store operations after a BF16 GEMM. Their gated residual semantics could share one implementation.

## What evidence was available?

The source required retaining the BF16 GEMM result before FP32 gate multiplication and residual addition, followed by the final store. Output aliases the old residual; timing without restoring it would accumulate values and change the comparison.

## What was the hypothesis?

Retain both GEMMs and fuse their seven following operations into one CUDA pass, using a small shared scratch allocation to remove traversals and launches.

## What commands and changes followed?

Run `python -m lab.pi05.rtx5090_fused_residual --seed 42`. Check actual calls 0/17/90/179 at both sites and restore the same residual on both paths. Integrate the shared implementation and two routes, then run official parity. Declaration checks also exposed early native loading in the backbone factory; e9875fa deferred it until first execution.

## What were the results?

Selected actual outputs were bitwise equal. Local output projection improved from 19.1919 to 7.7451 µs and FFN down from 22.8162 to 12.7092 µs; scratch grew by 100 KiB. Official parity passed, and deployment median fell from 41.0824 to 37.9806 ms. The scoped 8+1 declaration/route checks passed.

## Why retain, revert or investigate further?

Retain. This trial changed both sites using the shared residual kernel, so its full gain cannot be assigned to either site alone. GEMMs and numerical tolerances were unchanged. The lazy-loader fix changed no CUDA arithmetic and was not recorded as another performance point.

## How to inspect the process

The excerpt contains 42 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 18, "function_call": 16, "function_call_output": 16}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/rtx5090_fused_residual.md](evidence/lab/pi05/rtx5090_fused_residual.md)
