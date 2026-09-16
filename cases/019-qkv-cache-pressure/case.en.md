# 019 · Make L2 capacity explicit when screening the Expert QKV GEMM

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `0dcb021`.

Relate the working set to cache capacity, then measure local behavior and deployment benefit separately.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

The middle GEMM dominated QKV call time. The candidate replaces this GEMM while retaining the validated prepare and finish stages.

## What evidence was available?

The old GEMM already launched 160 CTAs on a GPU with 170 SMs, so insufficient blocks was not an established explanation. The 18 layer weights totaled only 90 MiB, below the RTX 5090 L2 capacity of 96 MiB; a local loop could keep weights hotter than deployment.

## What was the hypothesis?

Compare three fixed tile and reuse choices. Rotate two independent copies of the real weights, totaling 180 MiB, as a cheap check against an advantage that appears only with a warm working set. This experiment does not reproduce the deployment sequence.

## What commands and changes followed?

Run `lab.pi05.expert_qkv_matmul_screen`: check BF16 projected values, Q/K/V and factor per layer, then run two-copy GEMM ABBA. The winner proceeds to an original-18-layer full-chain ABBA. Deploy only 16×32×32 with 4 warps and 3 stages; follow official validation with full-model timing.

## What were the results?

All checked outputs across 18 layers were bitwise equal. The winner used 40 registers, 6 KiB shared memory and no spills. Under cache pressure, A 9.9929/9.9529 became B 8.5680/8.5698 µs; the original-weight full chain still showed substantial drift. Deployment A32.565772/32.616870 became B32.426883/32.429453 ms, improving mean medians by 0.163153 ms.

## Why retain, revert or investigate further?

Retain because the full-model candidate and control medians remained separated. Preserve the local full-chain drift of 0.4711/0.3093 µs rather than substitute the cleaner pure-GEMM graph. The two-copy result supports this candidate under cache pressure; it does not establish deployment cache hit rates or a complete causal explanation.

## How to inspect the process

The excerpt contains 100 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 16, "function_call_output": 16, "agent_message": 21}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/expert_qkv_matmul_screen.md](evidence/lab/pi05/expert_qkv_matmul_screen.md)
