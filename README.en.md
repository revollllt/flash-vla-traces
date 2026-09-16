# Flash-VLA Traces

[中文](README.md) | [English](README.en.md)

A record of evidence, attempts and decisions during optimization, intended to help other models learn how to advance a problem.

The [45-case index](index.en.md) currently links **26 cases with corresponding Chinese and English six-stage narratives**, ordered observable traces and supporting evidence. Suggested starting points are [001 basic fusion](cases/001-expert-ffn-fusion/case.en.md), [026 early screening](cases/026-vision-residual-source-screen/case.en.md), [018 withdrawal after inconclusive timing](cases/018-pv-inconclusive-revert/case.en.md) and [021 retention after a reverse-order check](cases/021-qkv-finish-reverse-order/case.en.md).

This update adds 14 cases, completing coverage of all 20 retained optimizations other than 023/024. Existing case 023 is preserved and translated; 024 is outside this expansion. Previously documented screening and diagnostic cases remain included.

## File organization

```text
index.md / index.en.md
cases/<id>-<topic>/
  case.md / case.en.md              # Corresponding Chinese/English narratives
  trace.jsonl                      # Original public updates and tool I/O
  evidence/README.md / README.en.md # Bilingual provenance guides
  evidence/…                       # Measurements, failures, patches, original notes
experiments/README.md / README.en.md # Bilingual student-experiment conventions
```

The existing filenames remain the Chinese versions; English files use `.en.md`, with links in both directions. The six-stage narratives, index, repository overview and evidence guides are bilingual. Traces, commands, measurements, patches, historical public updates and archived experiment documents are shared in their original language. English cases link to the original-update appendix; translations are not inserted into source events.

Each case follows **bottleneck → evidence available at the time → hypothesis → commands and changes → results → reason to retain, revert or investigate further**. The case pages are retrospective narratives; `trace.jsonl` contains excerpts of original observable events in order. Experiment documents may have had results appended later, so their conclusions must not be treated as prior agent knowledge. Timestamps help establish the order of public updates and tool operations.

## Provenance and limits

Source task: **优化 RTX 5090 上 Pi0.5 延迟**. The model was GPT-6 Astra (`gpt-6-astra`) with `xhigh` reasoning effort. This was an ordinary optimization run, without an additional requirement to collect teaching traces. The repository was assembled afterward.

The main visible execution window was 2026-09-15 02:20–08:18, Asia/Shanghai. Traces preserve the original UTC timestamps. Code started at `5ac75bc`; the committed history inspected during preservation was `21d95c3`. The final session stopped at the 429 retry limit, which does not establish that optimization opportunities were exhausted.

Workload: `kai0/pi05-belt-cup/orbax-39999+openpi-convert-pi05_aloha`, BF16, batch1, 3×224×224 images, 200 prompt slots, chunk50, 18 layers and 10 denoise steps. Timing used seed42; the original official comparison used a separately saved seed0 fixture. Hardware/software: RTX5090, 170 SMs, 96 MiB L2, driver580.142, torch2.13.0+cu130, Triton3.7.1 and native CUDA13.1, with unlocked clocks.

Deployment measurements used 5 warmup iterations and 100 samples after initial capture, a fresh process for each version, serial execution on the same GPU and no profiler. The measured scope includes input staging, host processing, graph replay and final synchronization, excluding model loading/capture. Later small-gain trials used ABBA and sometimes a predefined BAAB follow-up; these protocols must not be attributed retrospectively to earlier trials. Local warmup/repeat/cache/reset conditions are specified in their original records.

The recorded initial and final retained measurement points were 59.577903 and 30.185328 ms. They describe a cumulative run on a fixed workload, not a fair ranking against other open-source projects or evidence of robot task success. Official-oracle provenance includes a locally modified vendored OpenPI. The initial “torch” route also does not imply every internal computation was uncompiled.

Full original sessions remain private under `/Users/zou/.codex/sessions/2026/09/15/` and are not copied into this repository:

| Role | Session ID | Original file |
|---|---|---|
| main | `01a0a125-31f9-7be2-af47-1e480f38ca2d` | `rollout-2026-09-15T02-19-24-01a0a125-31f9-7be2-af47-1e480f38ca2d.jsonl` |
| ffn | `01a0a12d-5cb3-7a50-ba9a-f2075d1006c2` | `rollout-2026-09-15T02-28-19-01a0a12d-5cb3-7a50-ba9a-f2075d1006c2.jsonl` |
| qkv | `01a0a12d-af7f-7263-a778-f7c0dd627db9` | `rollout-2026-09-15T02-28-40-01a0a12d-af7f-7263-a778-f7c0dd627db9.jsonl` |
| backbone | `01a0a131-ce71-7062-bbb5-d4f41ff2d2cd` | `rollout-2026-09-15T02-33-11-01a0a131-ce71-7062-bbb5-d4f41ff2d2cd.jsonl` |

Code and results come from `yx5090:/home/ubuntu/flash-vla`. Its current worktree contains later edits and was not modified during preservation. Evidence guides list original paths; implementation patches come from the corresponding historical commits, rather than reconstructions from today’s worktree. No new GPU optimization or remeasurement was performed.

Original sessions contain sensitive inputs, encrypted communication and internal state. Only public assistant commentary/final messages and execution-tool inputs/outputs are exported. Original user inputs, internal reasoning, encrypted agent payloads and compacted state are excluded. Credentials and complete lines containing sudo-stdin credentials are redacted. Some subagent instructions are unreadable, so the repository does not claim to preserve complete delegation prompts. Explanatory inferences are annotations, not recovered private reasoning.

Each trace record contains `timestamp`, `source.agent/session_id/file/line` and `kind`; tool records additionally contain `call_id`. Parent and child excerpts use their own execution windows, excluding inherited early context. Related cases can cite the same original event; deduplicate by session_id+line when merging cases. Parallel windows retain context from adjacent tasks, and cross-task timestamps alone do not establish causality. Already-truncated tool returns are not reconstructed.

Attached material supports review of the main reported numbers and code changes; it is not a standalone runnable environment. Weights, original input safetensors, some binaries and complete build dependencies are not included. Historical paths and links remain as provenance and may not open directly from this repository. Exact reruns require the corresponding source-project commits and data.

## Maintenance and use

This is an independent local Git repository without a remote or submodule. Flash-VLA owns the implementation; this repository owns the cases. If executable reproduction is needed, this repository can later reference the appropriate Flash-VLA version. Do not expose the case repository by default in every student worktree, since a no-trace control could otherwise find it through search.

Historical GPU-slot coordination, pauses and authorization wording are part of the recorded experiment, not new instructions to a reader. Students should propose candidates and validate them against their own current workloads rather than inherit speedup claims. Minimal research comparison conventions are in [experiments/README.en.md](experiments/README.en.md).

Nineteen entries remain index-only, including 024, which was excluded from this expansion. The repository does not claim all 45 cases are complete. Material has not been published externally; licensing and citation conventions remain to be determined before publication.
