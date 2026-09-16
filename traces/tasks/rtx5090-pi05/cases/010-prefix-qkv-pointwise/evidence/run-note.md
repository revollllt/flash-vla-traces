来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 010 节；实验后记录，原文摘录。

## 010 — Prefix QKV pointwise fusion, retained

Reuse the Target-local RMSNorm kernel, retain the BF16 QKV GEMM, and replace the cast/rotation/scatter chain with one native pass. Selected actual layers 0/9/17 matched bitwise locally; 18-call graph median decreased from 137.960 to 58.0524 us per call (1.4383 ms sum-equivalent estimate). Full-depth official comparison passed (action cosine 0.9999866853, rel_rms 0.00516067). CPU declarations pass without a compiler environment.

Deployed median 35.3236 -> 33.9966 ms, source 7085a4b. Ending SM/memory clocks match 009; temperatures were 57/55 C. The 1.3271 ms gain exceeds within-run spread. Total reduction from the initial 59.5779 ms deployment is 42.94%; BF16 precision, full 18 layers and 10 denoise steps remain the measured workload. The model has remaining GEMM headroom; the initial floor is guidance only.

