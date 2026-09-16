来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 004 节；实验后记录，原文摘录。

## 004 — Packed expert FFN, retained

Hypothesis: pay skinny-GEMM launch/scheduling overhead once for gate and up. One BF16 GEMM writes two contiguous halves; native activation preserves BF16 projection rounding. Source tensors and packed weights are retained in the wrapper; all GPU storage uses runner scratch. Additional packed weights: 288 MiB for 18 layers.

Selected real invocation outputs/factors were bitwise equal; local median 25.8104 -> 17.1574 us/call, 1.5575 ms estimated across180 calls. Full-depth official comparison passed. Deployed shipped median 45.8682 -> 44.4914 ms, source ebfde75.

