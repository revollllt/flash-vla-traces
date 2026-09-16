# 013 · Share cfg10 across both Vision FFN GEMMs and retain the deployment drift range

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `5f7ccf0`.

When configurations are close, reuse one validated tile; control drift limits the precision of the conclusion.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

The two Vision FFN bias GEMMs still offered measurable headroom. The next question was whether their different shapes could reuse the existing CUTLASS screening interface.

## What evidence was available?

ldc=0 broadcasts BF16 bias into the FP32 accumulator before BF16 output; down additionally requires a separate BF16 residual add. Using separate cfg5/cfg10 winners was estimated to save only another 0.014 ms over a common cfg10. Initial error aggregation hit the quantile size limit at about 89 million elements; this was not a GEMM numerical failure.

## What was the hypothesis?

Sharing cfg10 could recover most of the local gain with a smaller production change. Check errors per layer, then time the complete norm/GEMM/activation or residual chain.

## What commands and changes followed?

Use `lab.pi05.cutlass_vision_screen` and `lab.pi05.cutlass_vision_chain`, changing error aggregation to call the existing metric function per layer. Select CTA64×128×32, warp32×64×32 and 5 stages in the same native library. Keep up normalization/GELU and down’s separate residual. After 27 real layers and repeated replay pass, switch both routes and run deployment ABBA.

## What were the results?

Compilation reported 160 registers and no spills. Conservative local separation across both sites totaled about 0.369 ms. Official action cosine was 0.9999915368 and rel_rms 0.00411417. Deployment A33.459378/33.579306 became B33.057243/33.062119 ms, with A/B drift 0.119928/0.004875 ms.

## Why retain, revert or investigate further?

Retain because every candidate median remained below every control. Report an observed improvement of roughly 0.40–0.52 ms rather than an overly precise single-kernel causal gain. Do not add repeats merely because control drift is visible, or another production template for about 0.014 ms of extra local potential.

## How to inspect the process

The excerpt contains 134 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 18, "function_call": 15, "function_call_output": 15}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/cutlass_vision_screen.md](evidence/lab/pi05/cutlass_vision_screen.md)
- [lab/pi05/cutlass_vision_chain.md](evidence/lab/pi05/cutlass_vision_chain.md)
