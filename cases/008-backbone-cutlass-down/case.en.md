# 008 · Deploy Backbone down separately while preserving C=D residual semantics

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `54b5946`.

Deploy different call sites separately even when they share an implementation, so cumulative gains are not counted twice.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Case 007 had deployed gate/up, while backbone down still used its original GEMM with a local total of about 5.8 ms.

## What evidence was available?

Preparation of cfg0 had already checked 17 real down calls. Unlike Expert down, the residual belongs inside this GEMM epilogue, requiring alpha=beta=1 and C=D rather than beta=0 followed by gating.

## What was the hypothesis?

Use the validated cfg0 Stream-K tile for down to obtain a separately measured increment while retaining its residual expression.

## What commands and changes followed?

Reuse case 007’s cutlass_backbone implementation and real-call probe. Each runner owns its workspace and pointer-bound plans, initialized during warmup. In 54b5946, switch only the down route, then run official parity and fresh-process latency commands. The implementation, routing patch and complete local report are attached.

## What were the results?

All 17 actual calls passed existing tolerances. Local totals improved from A5.827/5.869 to B5.038/5.039 ms. Official parity passed, and deployment median fell from 37.0332 to 36.2217 ms.

## Why retain, revert or investigate further?

Retain as the increment after accepted case 007. Do not compare again with 006 or the initial model and double-count earlier gains. The result supports this workload, residual expression and cfg0 scheduling.

## How to inspect the process

The excerpt contains 41 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 4, "function_call": 5, "function_call_output": 5}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
