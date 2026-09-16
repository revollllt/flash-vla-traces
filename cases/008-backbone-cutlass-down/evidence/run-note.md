来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 008 节；实验后记录，原文摘录。

## 008 — CUTLASS backbone down projection, retained

The same cfg0 Stream-K tile now runs the down projection with C=D and alpha=beta=1, preserving the existing residual expression. All17 actual calls passed existing tolerance; local total5.827/5.869 ->5.038/5.039ms. Full-depth official comparison passed.

Deployed median37.0332 ->36.2217ms, source54b5946. This is the incremental gain after007. Native workspace and pointer-bound plans are owned per runner and initialized during warmup.

