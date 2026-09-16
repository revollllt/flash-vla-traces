# 015 · Reuse Vision cfg10 through a wrapper and qualify the small observed gain

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `5772cf2`.

Cheap reuse can yield a small benefit, but limited samples and unlocked clocks still constrain its precision.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Vision QKV still used a bias-fused Torch GEMM. Existing cfg10 could directly test the new M768/K1152/N3456 shape.

## What evidence was available?

Both fused_vision LayerNorm and the cfg10 bias GEMM already existed. A Python wrapper could compose them without changing CUDA, tile or native ABI. The experimental worktree retained existing libraries matching its ABI.

## What was the hypothesis?

Test one already-built cfg10 at the new shape, retaining LayerNorm and input/output layout, to determine whether the full chain becomes faster.

## What commands and changes followed?

Run `python -m lab.sm120.pi05_vision_qkv_cutlass_probe`. Capture and compare all 27 actual layer outputs and run ABBA under the same graph-replay conditions. Add the wrapper in 9215399, select the route in 5772cf2, then follow official validation with four independent deployment processes.

## What were the results?

All 27 layer outputs were bitwise equal. Local ABBA was A1.010784/B0.962976/B0.964240/A1.026688 ms, with minimum separation 0.046544 ms. Deployment A32.849472/32.834406 became B32.807739/32.796503 ms, improving mean medians by 0.039818 ms. Official action metrics matched 014.

## Why retain, revert or investigate further?

Retain the small observed gain. A/B drift was 0.015066/0.011236 ms and minimum separation 0.026667 ms. The narrow margin does not support a more precise or universal claim. The original experiment did not add a reverse block; document that rather than retrospectively imply stronger validation.

## How to inspect the process

The excerpt contains 48 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 9, "function_call_output": 9, "agent_message": 2}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md](evidence/results/rtx5090-pi05/gpt6-vision-qkv-cfg10/README.md)
