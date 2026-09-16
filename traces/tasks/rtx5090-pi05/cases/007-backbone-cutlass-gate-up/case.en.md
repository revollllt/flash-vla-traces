# 007 · Screen existing CUTLASS configurations for large GEMMs while controlling the library revision

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `594e697`.

Reuse existing implementations and control version changes; tile rankings below observed drift do not justify a firm conclusion.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

After pointwise fusion, large backbone GEMMs became a major remaining cost. The question was whether existing CUTLASS scheduling could improve gate/up before investing in an unbounded tile search.

## What evidence was available?

The worker inspected 12 existing configurations. Gate/up screening rotated four real weight sets from the first two layers, totaling 256 MiB and exceeding the 96 MiB L2. Local improvement was supported, but cfg0 and cfg5 were too close relative to control drift. Two installed CUTLASS checkouts were subsequently found to have different revisions.

## What was the hypothesis?

Reuse the measured Stream-K cfg0 while keeping BF16 projection outputs and surrounding fusion unchanged. First retain the vendor revision used in screening so a library change is not mixed into the comparison.

## What commands and changes followed?

Prepare the `lab.pi05.cutlass_gemm_screen` probe and cutlass_backbone backend with CTA 128×128×64, warp 64×64×64 and 3 stages. Retain library revision cb4247394dd82148787aed73e5dc7cef33cbf862. Validate warmup, capture, replay, 17 real calls and lazy loading, then switch only the gate/up deployment route. Down integration belongs to case 008.

## What were the results?

Compilation reported 254 registers and no spills. The 17-layer FFN local totals were A 11.591/11.862 ms and B 10.866/10.883 ms; the official comparison passed. Deployment median fell from 37.9806 to 37.0332 ms.

## Why retain, revert or investigate further?

Retain one cfg0 without claiming it beats cfg5 or is globally optimal. The evidence supports this deployment replacement. Although down had also passed local validation, it was deployed and measured separately to keep the two increments distinct.

## How to inspect the process

The excerpt contains 88 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call_output": 19, "function_call": 18, "agent_message": 18}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
