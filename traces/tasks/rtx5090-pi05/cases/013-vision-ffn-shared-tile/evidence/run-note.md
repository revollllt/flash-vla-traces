来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 013 节；实验后记录，原文摘录。

## 013 — Shared vision FFN bias-GEMM tile, retained

Both vision FFN GEMMs use cfg10 (64x128x32, warp32x64x32, 5 stages) in the same Target-native library. The BF16 bias is read with ldc=0 and added to the FP32 accumulator before BF16 output. Up retains the existing norm and GELU; down retains the independent BF16 residual add. All 27 layers per site passed local tolerance, and repeated graph replay matched bitwise. Choosing separate cfg5/cfg10 tiles estimated only another 0.014 ms locally, so the production candidate uses one tile.

Complete-chain ABBA totals: up A1.398624/B1.298080/B1.297056/A1.425248 ms; down A1.394720/B1.116160/B1.112064/A1.384448 ms. Conservative local separation totals about 0.369 ms. Full-depth official comparison passed (action cosine 0.9999915368, rel_rms 0.00411417). The compiled new tile uses 160 registers and no spills.

Fresh-process end-to-end ABBA gives prior plan 33.459378/33.579306 ms and candidate 33.057243/33.062119 ms. A drift is 0.119928 ms versus B drift 0.004875 ms, so the observed improvement is reported as approximately 0.40–0.52 ms, not a falsely precise single kernel attribution. SM clocks ended 2857/2872/2865/2857 MHz, memory 13801 MHz, temperatures 59/60/59/58 C; unchanged power policy can couple workload to boost frequency. Only the two vision FFN routes differ. Every candidate/control median remains separated, and no extra repeat was added. Source 5f7ccf0.

