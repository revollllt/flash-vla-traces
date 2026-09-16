# 001 · Fuse the Expert FFN pointwise chain while retaining GEMMs

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `23f3c8b`.

Keep expensive operators and numerical boundaries fixed while testing surrounding fusion; distinguish local estimates from deployment results.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

The initial model median was 59.577903 ms. The action expert FFN accounted for about 8.559 ms and 3780 launches in the profiler, making its repeated short chains an early candidate.

## What evidence was available?

Each FFN call had about 21 launches: two GEMMs plus RMS, casts, bias, GELU and multiplication. After reporting the hotspots, the parent assigned FFN and QKV candidates to separate worktrees while scheduling GPU measurements serially. The FFN worker publicly identified the BF16 rounding of the RMS factor and successive input multiplications.

## What was the hypothesis?

Retain both torch.mm calls. Combine normalization and scaling in one CUDA kernel, and bias/GELU/product in another, to recover traversal and launch costs with a small change. Weight packing is a separate experiment.

## What commands and changes followed?

Implement fused_ffn and a real-call probe, entered through `python -m lab.pi05.rtx5090_fused_ffn --seed 42`; the attached original note and trace contain full arguments. Capture 180 actual calls, check outputs and factors at calls 0/17/90/179, then time the local graph. The parent integrates the route and runs official parity and `python -m benchmarks latency`. Both implementation and routing patches are attached.

## What were the results?

The checked outputs and RMS factors were bitwise equal. Local time fell from 54.9115 to 25.8879 µs per call. The full official numerical comparison passed; deployment median fell from 59.577903 to 55.4515 ms, about 4.1264 ms lower. Multiplying the local difference by 180 gives an estimated 5.224 ms, not a deployment measurement.

## Why retain, revert or investigate further?

Retain the candidate: the deployment difference was large relative to recorded within-run variation. This early case used one fresh process per version, without the ABBA used in later cases; it must not be described retrospectively as equally strong repeated validation. The power-limit flag and unlocked clocks leave the cause of the local-to-deployment gap unresolved.

## How to inspect the process

The excerpt contains 67 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 13, "function_call_output": 13, "agent_message": 13}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/rtx5090_fused_ffn.md](evidence/lab/pi05/rtx5090_fused_ffn.md)
