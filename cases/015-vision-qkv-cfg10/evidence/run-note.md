来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 015 节；实验后记录，原文摘录。

## 015 — Reuse vision bias-GEMM tile for QKV, retained

A small Python wrapper reuses the deployed cfg10 native bias GEMM and existing LayerNorm for vision QKV. No new CUDA or ABI is introduced. All 27 actual-layer outputs match bitwise; local ABBA totals are 1.010784/0.962976/0.964240/1.026688 ms, with only 0.046544 ms minimum separation. Raw local evidence is in results/rtx5090-pi05/gpt6-vision-qkv-cfg10. Full-depth official comparison passed with action metrics equal to 014.

Fresh-process end-to-end ABBA gives prior plan 32.849472/32.834406 ms and candidate 32.807739/32.796503 ms. The mean of medians improves by 0.039818 ms; A/B drift is 0.015066/0.011236 ms. Both candidate medians are below both control medians, with the nearest separation 0.026667 ms. This supports retaining a small observed gain under the existing unlocked-clock conditions; its exact size is uncertain and no extra repeat was added. Only vision_encoder_norm_qkv differs between loaded plans. SM clocks ended 2872/2865/2865/2865 MHz, memory 13801 MHz, temperatures 58/58/59/58 C. Source 5772cf2.

