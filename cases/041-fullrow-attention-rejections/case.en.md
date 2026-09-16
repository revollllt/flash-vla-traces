# 041 · Reject two full-row attention mappings despite lower resource usage

[中文](case.md) | [English](case.en.md)

Outcome: **Both fixed mappings rejected**. Historical code reference: `c6a7d56 / 0325a34`.

Use resource observations to propose the next hypothesis and complete-chain comparisons to decide whether to retain it.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Backbone attention still used two GEMMs, softmax and intermediate matrices. The experiment kept entire score/probability rows inside a CTA while preserving both BF16 intermediate rounding boundaries.

## What evidence was available?

The original chain accounted for about 1.122 ms across 17 calls in the profiler; softmax/copy contributed only about 0.166 ms. CPU analysis enumerated register payload, repeated K/V reads and gather-layout costs that could outweigh eliminating global intermediates.

## What was the hypothesis?

First fix BM32, 8 warps, one stage and QK BK32/PV BK64 to test net complete-chain benefit. After BM32 failed, change only to BM16 to test lower resource pressure against doubled repeated reads. Do not pre-attribute BM32’s slowdown to spills.

## What commands and changes followed?

Use `lab.pi05.backbone_fullrow resources/prepare/check/time`: inspect zero-input compiled resources and PTX, capture 17 real layer inputs, validate numerics, then run one ABBA. BM16 reuses the same snapshot, equal Q reset and timing protocol, saving separate files. Neither trial changes the production attention route.

## What were the results?

BM32 used 255 registers, 104 B/thread local footprint and 66 KiB shared memory; numerics passed, but the local chain was 0.579184 ms slower per 17 calls. BM16 reduced this to 206 registers, 0 B local and 65 KiB shared, also passed numerics, yet was 0.684042 ms slower than its own ABBA control. Both differences clearly exceeded within-trial drift.

## Why retain, revert or investigate further?

Reject both fixed mappings and retain all raw samples. Removing local footprint in BM16 did not produce a gain, showing that a single resource metric cannot predict latency. The experiment did not isolate gather, repeated reads or scheduling costs. The tiles ran in separate timing windows, so their absolute candidate-time difference is not a strict controlled causal comparison.

## How to inspect the process

The excerpt contains 109 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 25, "function_call_output": 25, "agent_message": 27}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [lab/pi05/backbone_fullrow.md](evidence/lab/pi05/backbone_fullrow.md)
