# 009 · Fuse Vision LayerNorm/GELU while retaining centered variance and rounding

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `cc5f0a8`.

Check variance computation and BF16 boundaries before fusing nonlinear operations; matching operation names is not sufficient.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

In profile 005, vision norm_ffn_up used 2.055 ms/270 launches and norm_qkv used 1.249 ms/162 launches, with repeated casts and traversals.

## What evidence was available?

The GEMMs already fused bias. LayerNorm used an FP32 mean and centered variance, retaining FP32 gamma/beta until the final normalized store. GELU had to read the projection after BF16 rounding rather than operate directly on GEMM accumulators.

## What was the hypothesis?

Implement FP32 LayerNorm→BF16 in one CUDA pass and apply in-place tanh GELU after the BF16 projection. Retain both bias-fused torch.addmm calls.

## What commands and changes followed?

Run `python -m lab.sm120.pi05_vision_fusion_probe`. Capture 27 actual layers and check layers 0/13/26, restoring reference outputs before advancing. Time both full call chains while preserving lazy native loading and runner scratch. After integration, run official parity, CPU Target declarations and deployment measurements.

## What were the results?

Selected normalized values and outputs passed tolerances. The 27-layer local ABBA suggested about 1.07 ms of combined headroom. Official action cosine was 0.9999852922 and rel_rms 0.00542377. Deployment median fell from 36.2217 to 35.3236 ms, a 0.8981 ms reduction.

## Why retain, revert or investigate further?

Retain. Both deployment runs ended with matching SM/memory clocks, temperature and power-limit reason; within-run variation was much smaller than the difference. Endpoint observations do not establish identical frequency histories, and the local estimate does not replace the deployment result.

## How to inspect the process

The excerpt contains 65 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 15, "function_call_output": 15, "agent_message": 11}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [results/rtx5090-pi05/gpt6-vision/README.md](evidence/results/rtx5090-pi05/gpt6-vision/README.md)
