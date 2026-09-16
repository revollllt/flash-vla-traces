来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 017 节；实验后记录，原文摘录。

## 017 — Reuse vision bias GEMM for output projection, retained

Reuse cfg10 for M768/K1152/N1152 with bias, then retain the separate BF16 residual add. The existing residual wrapper takes K from the weight shape and serves both vision output and FFN down projection. No new native kernel or ABI is added. All 27 actual layers pass existing shallow tolerance (worst rel_rms 0.00105418, minimum cosine 0.999999444); these outputs are not bitwise equal to cuBLAS, while repeated candidate replay is identical. Local complete-chain ABBA gives A0.587776/B0.454656/B0.454656/A0.587776 ms, or 0.133120 ms separation. The 68.34375 MiB weight set can fit L2, so deployment was measured independently.

Full-depth official comparison passed (action cosine 0.9999912284, rel_rms 0.00418872). Fresh-process end-to-end ABBA gives prior plan 32.771943/32.782361 ms and candidate 32.610786/32.613452 ms. The mean of medians improves by 0.165034 ms; A/B drift is 0.010418/0.002666 ms. Only vision_encoder_out_proj_residual differs between the loaded plans. Ending SM clocks are 2865/2872/2865/2865 MHz, memory 13801 MHz and temperatures 58/59/59/58 C. Source eb8c16a.

