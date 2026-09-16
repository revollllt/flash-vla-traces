# 005 · Preserve FP32 Expert attention scores while fusing scale, mask and softmax

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `4d53ad7`.

Preserve numerical boundaries and alias behavior, and restore inputs identically on both timing paths.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Expert attention Q/K conversions, FP32 SIMT GEMM and pointwise softmax work accounted for about 5.623 ms and 2160 launches in the preceding profile.

## What evidence was available?

The installed Torch interface supported BF16 inputs with FP32 matrix-multiplication output, which was first verified on the GPU. Pi0 mask and score-rounding semantics differed and could not simply be reused. The original call also allowed out=Q, so repeated replay could overwrite the next input.

## What was the hypothesis?

Use Tensor Core QK with BF16 inputs and FP32 logits; fuse scaling, runtime additive mask and stable softmax, then store BF16 probabilities at the original boundary. Retain the existing PV GEMM.

## What commands and changes followed?

Run `python -m lab.sm120.pi05_attention_fusion_probe`. The first build lacked math_constants.h; add that include and continue. With synthetic actual-shape inputs, check mask changes, input scales, out=Q and independence from masked V rows. Include the same Q reset in both local timing paths. Integrate 80cf420, select the route in 4d53ad7 and run official parity.

## What were the results?

Increasing masked V values did not change the output. Local checks passed existing tolerances, with worst output rel_rms about 1.56e-4. The reset-inclusive local chain improved from about 37.8 to 15.2 µs. Real-weight official parity passed, and deployment median fell from 44.4914 to 41.0824 ms.

## Why retain, revert or investigate further?

Retain this Expert attention chain. BF16-input QK with FP32 scores can change reduction order; output is not bitwise equal for every input. The earlier default-SDPA rejection concerns a different dispatch screen and does not reject this implementation or fused attention in general.

## How to inspect the process

The excerpt contains 39 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 6, "function_call": 8, "function_call_output": 8}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [results/rtx5090-pi05/gpt6-attention/README.md](evidence/results/rtx5090-pi05/gpt6-attention/README.md)
