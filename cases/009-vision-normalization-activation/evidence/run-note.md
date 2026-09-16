来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 009 节；实验后记录，原文摘录。

## 009 — Vision normalization and activation, retained

FP32 centered-variance LayerNorm now uses a single CUDA pass, retaining BF16 normalized inputs to unchanged bias-fused GEMMs. FFN GELU runs in place after the projection rounds to BF16. Selected real layers passed local tolerance; 27-layer ABBA estimates about 1.07 ms of headroom. Full-depth official comparison passed (action cosine 0.9999852922, rel_rms 0.00542377). CPU Target binding also passes without a CUDA compiler environment.

Deployed median 36.2217 -> 35.3236 ms, source cc5f0a8. Both measurements ended at 2865 MHz SM, 13801 MHz memory and 57 C with the same power clock reason; observed within-run variation is much smaller than the 0.8981 ms gain.

