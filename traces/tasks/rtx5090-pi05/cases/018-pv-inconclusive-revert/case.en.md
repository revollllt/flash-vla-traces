# 018 · Withdraw a locally faster PV candidate after deployment medians overlap

[中文](case.md) | [English](case.en.md)

Outcome: **Inconclusive; withdrawn**. Historical code reference: `1542c05`.

A small positive mean is not sufficient evidence of a stable gain; predefine follow-up measurements and allow an inconclusive outcome.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Expert attention PV used a GEMM and split-K reduction. A single-launch PV implementation was a small candidate with local potential.

## What evidence was available?

Nine real step/layer pairs passed existing tolerances. The 16×32×64 Triton candidate preserved FP32 logits, the runtime mask, materialized BF16 probabilities and out=Q. Local nine-call ABBA improved from A 50.786/51.024 to B 43.946/43.915 µs.

## What was the hypothesis?

Replace only PV and test whether the net saving after eliminating reduction can be distinguished from full-model measurement noise. Local improvement alone is not the final deployment criterion.

## What commands and changes followed?

Integrate the single PV backend, pass official parity and run fresh-process ABBA. When improvement was close to drift, declare exactly one reverse-order BAAB block before reading its results and retain all eight measurements. The full commands, public follow-up declaration, decision JSON and withdrawal patch are saved.

## What were the results?

ABBA was A32.596582/B32.499377/B32.533664/A32.561475 ms, a mean difference of 0.062508 ms. BAAB was B32.489375/A32.557318/A32.638460/B32.602458 ms, still favoring the candidate by 0.051972 ms on average, but with overlapping medians and A/B drift of 0.081142/0.113082 ms. All eight medians favored the candidate by about 0.057240 ms on average.

## Why retain, revert or investigate further?

Withdraw the production module, registration and route. The conclusion is that the gain is inconclusive under these measurement conditions, not that PV fusion is necessarily useless or slower. Clock changes were recorded, but endpoint readings did not isolate their causal contribution. No further repeats were used to seek a favorable result.

## How to inspect the process

The excerpt contains 133 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 23, "function_call": 21, "function_call_output": 21}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
