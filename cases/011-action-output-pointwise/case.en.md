# 011 · Use fresh ABBA controls to confirm a small action-output fusion gain

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `853d1fa`.

Remeasure the current control for small expected gains and explicitly restore inputs to state updates.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

The final action-output head accounted for 0.217349 ms and 170 kernels over 10 calls, about 17 launches per step, leaving a relatively small opportunity.

## What evidence was available?

The existing Target-local QKV library could reuse its loader and scratch. The public norm_factor argument was originally untouched. Euler updates mutate the action, requiring identical initial-value restoration in both timing paths. The ten-step local working set was about 1.745 MB and reused warm cache.

## What was the hypothesis?

Add two dedicated kernels around the unchanged BF16 GEMM: a private BF16 RMS factor and sequential FP32 factor/bias/Euler residual update. This could reduce about 17 launches to 3 while retaining state semantics.

## What commands and changes followed?

Run `python -m lab.sm120.pi05_action_out_fusion_probe`. Check all 10 actual-step outputs and the untouched factor buffer, then run local ABBA with equal resets. After official parity, use four fresh deployment processes in A-B-B-A order rather than the older 010 point as the control.

## What were the results?

All ten outputs and preserved factors were bitwise equal. Local Torch totals 0.234208/0.234160 became B0.066256/0.066336 ms. Deployment A33.956571/33.942453 became B33.743207/33.766837 ms, improving mean medians by 0.194490 ms with A/B drift 0.014118/0.023630 ms. Official action metrics matched 010.

## Why retain, revert or investigate further?

Retain this repeatable small gain under unlocked clocks. Remeasure the complete control route map instead of using a historical curve point, reducing the risk of attributing temporal drift to optimization. Warm-cache local timings do not describe a cold-weight scenario.

## How to inspect the process

The excerpt contains 75 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 11, "function_call": 10, "function_call_output": 10}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [results/rtx5090-pi05/gpt6-action-out/README.md](evidence/results/rtx5090-pi05/gpt6-action-out/README.md)
