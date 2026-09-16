# 016 · Fuse two FFN projections with BF16 rounding preserved, then measure the smaller deployment gain

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `78c8ca1`.

Let semantic checks guide implementation choice, and acknowledge where local extrapolation fails.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

The packed Expert FFN still wrote the GEMM output and read it again in a separate bias/GELU/product kernel.

## What evidence was available?

The public analysis estimated prepare/GEMM/postprocessing at about 1.44/12.01/2.32 µs per call. The available CUTLASS DualGemm placed bias and BF16 rounding in a different order, so it was not a drop-in replacement based on its name.

## What was the hypothesis?

A small Triton dual-dot kernel could round each FP32 accumulator through BF16 before the original FP32 bias/GELU/product and avoid intermediate storage. Screen only three tiles tied to specific layout questions.

## What commands and changes followed?

Run `lab.pi05.expert_dual_dot_screen` on 18 real layer snapshots and a 288 MiB rotating weight set exceeding L2. Check outputs, PTX rounding boundaries and compiled resources. Deploy only the 16×64×32 winner with 4 warps and 3 stages, remove the 819,200-byte projected scratch, and retain prepare. Follow with official parity and end-to-end ABBA.

## What were the results?

The 18 local layer outputs were bitwise equal. Resources were 64 registers, 18,432 B shared memory and no spills. Local A 16.0462/16.1280 became B 14.3022/14.3253 µs, suggesting 0.31–0.33 ms. Deployment A 32.861063/32.857811 became B 32.743964/32.777847 ms: the observed mean improvement was 0.098532 ms. Official metrics matched the preceding version.

## Why retain, revert or investigate further?

Both candidate medians were below both controls, so the change was retained. Report the actual deployment gain without collecting more runs to seek a larger value. Endpoint power and clock readings do not explain why local gains shrank; cache, temperature and frequency contributions were not independently isolated.

## How to inspect the process

The excerpt contains 120 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 19, "function_call": 20, "function_call_output": 20}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/expert_dual_dot_screen.md](evidence/lab/pi05/expert_dual_dot_screen.md)
