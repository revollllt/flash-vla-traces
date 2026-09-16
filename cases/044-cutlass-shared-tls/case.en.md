# 044 · Isolate a shared-TLS initialization failure with standalone and two-library probes

[中文](case.md) | [English](case.en.md)

Outcome: **Fault isolated; probe corrected**. Historical code reference: `7085a4b (deployed at the time)`.

When standalone execution passes but coexistence fails, control the shared state first; a diagnostic fix need not become a runtime workaround.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

While reusing existing CUTLASS configurations to screen Expert GEMMs, cfg0 failed to launch in the full runner. Performance could not yet be measured.

## What evidence was available?

The same 50×1024×8192 shape passed in a standalone process. Attributing the failure directly to the shape or tile would ignore the environment difference between full-runner and single-library execution.

## What was the hypothesis?

Two dynamic libraries might share template initialization state without configuring both CUDA kernel entries for dynamic shared-memory opt-in. Construct a minimal coexistence probe before changing the tile.

## What commands and changes followed?

Preserve the original status719 log. Run standalone, two-library and diagnostic TLS-reset probes. Inspect the address of GNU-unique GemmUniversalBase::device_ordinal_; set it to -1 only inside the diagnostic process and check whether the second library initializes. Then make the cfg0 probe reuse the deployed Pi0.5 library, keeping the old interface for other configurations.

## What were the results?

The libraries shared the TLS address. Initializing one caused the other to skip cudaFuncSetAttribute. Resetting it diagnostically made the second library run, supporting the shared-initialization diagnosis. Reusing the deployed library allowed all 12 configurations to pass and identified cfg9 as a small local candidate.

## Why retain, revert or investigate further?

Retain the probe fix without adding runtime TLS resets to production. This case resolves experimental infrastructure and has no deployment speedup claim of its own. Complete-chain and deployment benefits of cfg9 are addressed by later experiments.

## How to inspect the process

The excerpt contains 102 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 16, "function_call_output": 16, "agent_message": 17}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [lab/pi05/cutlass_expert_screen.md](evidence/lab/pi05/cutlass_expert_screen.md)
