# 045 · Interpret low occupancy using Tensor activity and same-frequency throughput

[中文](case.md) | [English](case.en.md)

Outcome: **Optimization priorities revised**. Historical code reference: `After 7085a4b, during stage 011`.

Interpret resources, activity, frequency and executed work together; profiling locates limits but does not replace deployment timing.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

After large-GEMM optimization, backbone remained the main hotspot. High register use, one resident CTA and low occupancy could appear to justify further parallelism tuning.

## What evidence was available?

Gate/up used 254 registers, 96 KiB dynamic shared memory and one 128-thread CTA per SM. The experiment selected a few relevant NCU metrics and reused real layer0 inputs and the deployed library. Settings were cache-control all, clock-control none and kernel replay: an isolated cold-replay diagnostic.

## What was the hypothesis?

Distinguish low occupancy from idle Tensor units. If Tensor work is already near the measured same-frequency limit, increasing occupancy alone may not help. Down has a different shape, warranting its own sample instead of extrapolating from gate.

## What commands and changes followed?

Use the prepare/profile paths in `lab.sm120.pi05_backbone_gate_ncu` and NVTX to select one GEMM. For down, restore the residual outside the measured range. Normalize throughput using Tensor/DRAM/L2 activity, effective SM frequency, useful work and actual M-tile padding. Historical sudo lines containing credentials are redacted; credential-free sampling parameters and CSVs remain.

## What were the results?

Gate Tensor activity was 92.06%, DRAM 14.63% and L2 49.03%; down was 95.90%/20.36%/50.24%. At the same capture’s 2.725260 GHz, down useful-work throughput reached 90.33% of the measured unit limit. Accounting for M968 being tiled to 1024 rows gave 95.56%, close to Tensor activity.

## Why retain, revert or investigate further?

Do not repeat the same 12-tile sweep at this point; prioritize other shapes and fusion boundaries. The observations support Tensor work as the main limit of these current large GEMMs, not exhaustion of all optimization possibilities. Cold-replay profiler duration is not deployment latency, and padding overhead is not necessarily recoverable time.

## How to inspect the process

The excerpt contains 167 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 16, "function_call": 18, "function_call_output": 18}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [results/rtx5090-pi05/gpt6-backbone-ncu-prep/README.md](evidence/results/rtx5090-pi05/gpt6-backbone-ncu-prep/README.md)
