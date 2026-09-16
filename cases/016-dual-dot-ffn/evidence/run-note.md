来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 016 节；实验后记录，原文摘录。

## 016 — Rounded dual-dot expert FFN suffix, retained

One short Triton kernel replaces the packed BF16 GEMM plus bias/GELU/product kernel. Prepare and packed weight ownership are retained. The sole production tile is 16x64x32 with four warps and three stages; the other two experimental tiles are not deployed. Both FP32 accumulators explicitly round to BF16 and back before FP32 bias, GELU and product. The 819,200-byte projection scratch is removed. All 18 real layer snapshots match bitwise locally. The 288 MiB rotating weight set exceeds L2. PTX confirms BF16-input/FP32-accumulator MMA, both BF16 roundtrips before bias, and separate outer multiply/add operations. The tile uses 64 registers, 18,432 bytes shared memory and no spills.

Local suffix ABBA gives A16.0462/16.1280 us and B14.3022/14.3253 us per call, suggesting 0.31–0.33 ms over 180 calls. Full-depth official comparison passed with action metrics equal to 015. Fresh-process end-to-end ABBA gives prior plan 32.861063/32.857811 ms and candidate 32.743964/32.777847 ms. The mean of medians improves by 0.098532 ms; A/B drift is 0.003253/0.033883 ms. Only action_expert_norm_gated_ffn differs. The deployment gain is smaller than the local estimate; no additional repeats were used to seek a larger result.

Ending SM clocks are 2872/2865/2865/2865 MHz, memory 13801 MHz, temperatures 57/59/59/57 C and power 586.45/598.67/597.60/588.57 W. Power and thermal behavior may interact with the unlocked clock policy, but these snapshots do not establish why the local gain shrank. The shipped route is retained based on the separated end-to-end medians. CPU backend declaration/route checks passed 9/9 without CUDA initialization before integration. Source 78c8ca1.

