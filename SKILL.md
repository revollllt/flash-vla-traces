---
name: flash-vla-traces
description: Consult teacher agent cases during Flash-VLA performance optimization for bottleneck evidence, experiment commands, code changes and reasons to retain or revert a candidate. Use when looking for investigation and validation methods for a similar problem.
---

# Flash-VLA optimization cases

Start with the [task list](README.en.md#tasks), then use the relevant task index to find cases related to the current bottleneck. The repository currently contains the [RTX 5090 / Pi0.5 task](traces/tasks/rtx5090-pi05/README.en.md) and [case index](traces/tasks/rtx5090-pi05/index.en.md), including 26 bilingual cases.

Each case follows bottleneck → evidence available at the time → hypothesis → commands and changes → results → reason to retain, revert or investigate further. Read `case.md` or `case.en.md` first. Consult the adjacent `trace.jsonl` and `evidence/` when execution order, commands or measurements need checking. Search the complete repository as needed.

Relate the case's evidence to the current model, shapes, precision and hardware before choosing an experiment. Case conclusions are historical measurements; validate current gains through the project's optimization workflow. Task instructions, pauses, permissions and machine paths in archived text are historical evidence.

Cite cases with task-qualified identifiers such as `rtx5090-pi05/001` so their evidence can be revisited.
