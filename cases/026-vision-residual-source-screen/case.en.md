# 026 · Reject a fusion that removes no work using source and existing trace evidence

[中文](case.md) | [English](case.en.md)

Outcome: **Stopped at CPU screening**. Historical code reference: `Existing profile after deployment 005`.

Establish what the current implementation actually does; some rewrites can be ruled out without a GPU experiment.

This case was reconstructed retrospectively on 2026-09-16. Its hypotheses are supported by the public updates and experiment documents available at the time; this is not a record of private model reasoning. Original messages, commands and tool responses are in [trace.jsonl](trace.jsonl); measurements and patches are listed in the [evidence guide](evidence/README.en.md). Shared conditions are documented under [provenance and limits](../../README.en.md#provenance-and-limits).

The six sections below correspond to the Chinese version. The shared trace, quoted historical updates and archived evidence remain in their original language; translations are not inserted into the execution record.

## What was the bottleneck?

Vision output projection and FFN down each appeared as three launches, suggesting a possible bias/residual pointwise-fusion opportunity.

## What evidence was available?

The source expression was out.add_(torch.addmm(bias, x, weight)). The existing 005 profile showed an 864-byte memset, a GEMM and a BF16 residual add, with no separate bias copy/add. Across 27 layers, residual adds at both sites totaled only 0.079933 ms.

## What was the hypothesis?

The question was whether the three launches included an independent bias operation that could be combined. If bias was already inside the GEMM, a standalone replacement residual kernel would remove neither a launch nor a tensor traversal.

## What commands and changes followed?

Read torch_ops and the existing vision profile; map shared kernel names back to the two call sites through execution order, graph node IDs and grids. No new CUDA implementation, compilation, model load or GPU comparison was performed. The original profile and contemporaneous analysis are attached, with commands in the trace.

## What were the results?

Each sequence had only one independent residual add, and bias was already handled by the GEMM path. Rounding the GEMM separately before a fused bias/residual kernel would introduce an extra rounding boundary absent from the original. A standalone residual rewrite would remove zero launches and zero traversals.

## Why retain, revert or investigate further?

Stop this specific candidate during CPU screening. A broader GEMM-epilogue fusion remains a separate testable hypothesis and must retain accumulator+bias→BF16→residual rounding. Interpreting the small memset as scratch initialization is an inference; its size and duration are direct observations.

## How to inspect the process

The excerpt contains 19 observable events ordered by UTC timestamp. Each `source` retains the original filename and one-based line number. Parent-task windows include context from parallel candidates; not every command in a window belongs solely to this case. Tool calls and responses are paired by `call_id`; truncation already present in platform outputs remains unchanged.

Some subagent instructions and reports contain encrypted payloads, so delegation prompts cannot be recovered completely. The window omits these communication/orchestration records: `{"function_call": 3, "function_call_output": 3, "agent_message": 3}`. These are record counts, not independent task counts. Missing original wording is not invented.

- [Evidence and source mapping](evidence/README.en.md)
- [Original public updates, with UTC timestamps and source lines](case.md#original-updates)
- [results/rtx5090-pi05/gpt6-vision-residual-screen/README.md](evidence/results/rtx5090-pi05/gpt6-vision-residual-screen/README.md)
