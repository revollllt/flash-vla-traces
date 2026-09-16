# 012 · Fuse a rounded gated epilogue and isolate a coverage bug with constant probes

[中文](case.md) | [English](case.en.md)

Outcome: **Retained after repair**. Historical code reference: `df996b8`.

Reduce a large numerical failure to a cheap coverage test; preserve the failed path as part of the case.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Expert down still needed a separate gated residual after projection. The goal was to fold it into the GEMM epilogue while preserving intermediate BF16 rounding and in-place residual semantics.

## What evidence was available?

The existing postprocessing kernels totaled about 0.185 ms. Earlier cfg9 GEMM screening also showed a small opportunity, but the two estimates could not simply be added. The CUTLASS broadcast epilogue applies output operations after partial accumulation is reduced, allowing BF16(acc) → FP32 gate multiply → FP32 residual add → BF16 store.

## What was the hypothesis?

A rounded cfg9 epilogue might remove temporary storage and a launch. When the first real-input numerical test failed severely, the immediate problem became implementation debugging. The error could not be treated as normal reduction-order variation, and timing had to stop.

## What commands and changes followed?

Run the real-call check through `python -m lab.pi05.rtx5090_expert_epilogue --seed 42` and stop on failure. Then check all 50 rows with residual-only, all-ones-product and column-varying-gate probes. Correct first-row values but unwritten later fragments motivated comparing ordinary and broadcast reduce iterators. A Target-local adapter added the missing destination fragment advance; constant coverage, 180 real calls and ABBA with equal residual resets were repeated.

## What were the results?

All three repaired constant probes were exact across all rows. The 180 real calls passed existing tolerances, and three same-mainloop rounding decompositions were exact. Local A 12.72080/12.71182 became B 10.67627/10.68071 µs. Official parity passed. Deployment A 33.784778/33.769896 became B 33.499048/33.482750 ms, a 0.286438 ms mean-of-medians improvement.

## Why retain, revert or investigate further?

Retain the repaired candidate and the failure evidence. The iterator diagnosis is supported jointly by source differences, constant coverage and post-fix results. Performance changed both the GEMM implementation and its epilogue; the entire gain cannot be attributed to removing the postprocessing kernel.

## How to inspect the process

The excerpt contains 184 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 34, "function_call_output": 34, "agent_message": 33}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/rtx5090_expert_epilogue.md](evidence/lab/pi05/rtx5090_expert_epilogue.md)
