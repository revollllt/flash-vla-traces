# 004 · Pack Expert gate/up into one GEMM and report the memory cost

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `ebfde75`.

Change GEMM organization separately and report the storage cost of packed weights.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

After case 001 removed many pointwise launches, Expert gate and up still used separate skinny GEMMs. Their two launch and scheduling costs became the next candidate.

## What evidence was available?

The projections share an input. Concatenating their weights by column lets one BF16 GEMM write two contiguous output regions while preserving projection rounding before activation. Packed weights for 18 layers require an additional 288 MiB.

## What was the hypothesis?

Pay launch and scheduling overhead once for gate/up rather than twice. Test this separately from the earlier pointwise fusion.

## What commands and changes followed?

Extend the packed candidate and probe in `lab.pi05.rtx5090_fused_ffn`. The wrapper retains original and packed weights; runtime buffers belong to runner scratch. Check selected real calls, time the same 180-call sequence, then select the deployment route in ebfde75 and run official parity.

## What were the results?

Selected real outputs and factors were bitwise equal. Local time fell from 25.8104 to 17.1574 µs per call, an extrapolated 1.5575 ms across 180 calls. Full official validation passed, and deployment median fell from 45.8682 to 44.4914 ms.

## Why retain, revert or investigate further?

Retain with the extra 288 MiB weight-storage cost explicit. A call-count extrapolation and an observed deployment difference are different quantities. This result does not establish that gate/up packing helps at every shape.

## How to inspect the process

The excerpt contains 25 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 5, "function_call": 7, "function_call_output": 7}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
