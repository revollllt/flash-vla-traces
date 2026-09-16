# Flash-VLA Traces

[中文](README.md) | [English](README.en.md)

Evidence, attempts and decisions from optimization, organized by task to help other models learn how to advance a problem.

## Tasks

| Task | Hardware / model | Teacher | Cases |
|---|---|---|---|
| [rtx5090-pi05](traces/tasks/rtx5090-pi05/README.en.md) | RTX 5090 / Pi0.5 | GPT-6 Astra | 45 index entries, 26 bilingual cases; [English index](traces/tasks/rtx5090-pi05/index.en.md) · [中文](traces/tasks/rtx5090-pi05/index.md) |

## Layout

```text
traces/
  tasks/
    rtx5090-pi05/
      README.md / README.en.md       # Task conditions, provenance and limits
      index.md / index.en.md         # Cases for this task
      cases/
        001-expert-ffn-fusion/
          case.md / case.en.md      # Chinese/English six-stage narratives
          trace.jsonl               # Original observable event excerpts
          evidence/                 # Measurements, patches and provenance
experiments/
  README.md / README.en.md           # Student-experiment conventions across tasks
```

Add future tasks under `traces/tasks/<task-id>/`, each with its own overview, index and cases, then add an entry to the task table above. Case numbers are local to a task; use a qualified identifier such as `rtx5090-pi05/001` when referring across tasks.

Each case follows bottleneck → evidence available at the time → hypothesis → commands and changes → results → reason to retain, revert or investigate further. Chinese and English narratives correspond section by section; traces, commands and evidence share the original text. Each task records its actual environment, code revisions, missing information and limits on its conclusions.

This is an independent local Git repository without a remote or submodule. Flash-VLA owns implementation; this repository owns cases. Appropriate code versions can be referenced later for executable reproduction. Material access and comparisons in student experiments are described in the [usage conventions](experiments/README.en.md).
