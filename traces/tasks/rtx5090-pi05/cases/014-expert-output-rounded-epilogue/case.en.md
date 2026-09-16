# 014 · Reuse the validated rounded epilogue for Expert output projection

[中文](case.md) | [English](case.en.md)

Outcome: **Retained**. Historical code reference: `7669007`.

Reusing an implementation still requires validation at the new shape, particularly when the local weights fit in L2.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Expert output projection still had a separate gated residual. The rounded cfg9 epilogue validated for FFN down in case 012 might also serve this site.

## What evidence was available?

The new site has K=2048 versus K=4096 for FFN down. Passing K through the workspace/plan ABI allows reuse. C=D and the sequence BF16 projection then FP32 gate multiply/add remain. The 18 weight sets total 72 MiB and can fit in the 96 MiB L2.

## What was the hypothesis?

Support the new K with the same kernel to remove one postprocessing pass without adding a second kernel. Confirm local savings independently in the model.

## What commands and changes followed?

Run `lab.pi05.rtx5090_expert_epilogue` with `--site action_expert_out_proj_residual`. Check constant coverage over all 50 rows, 180 actual calls and same-mainloop BF16-projection decompositions, restoring the residual equally on both paths. After official parity, switch only this route and run deployment ABBA.

## What were the results?

Constant probes and selected rounding decompositions were exact. All 180 calls passed tolerance, with worst rel_rms 0.00194467. Local A9.08560/9.08836 became B7.40080/7.40196 µs. Official parity passed; deployment A33.139814/33.110785 became B32.845290/32.833674 ms, a mean-of-medians gain of 0.285817 ms.

## Why retain, revert or investigate further?

Retain. Deployment A/B drift was 0.029029/0.011616 ms and medians stayed separated. Warm-working-set local results were not used as proof of deployment benefit. The full improvement cannot be assigned solely to removing a residual kernel because the GEMM path also changed.

## How to inspect the process

The excerpt contains 88 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"agent_message": 10, "function_call": 13, "function_call_output": 13}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [Archived experiment summary](evidence/run-note.md)
- [lab/pi05/rtx5090_expert_epilogue.md](evidence/lab/pi05/rtx5090_expert_epilogue.md)
