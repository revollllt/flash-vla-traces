来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 014 节；实验后记录，原文摘录。

## 014 — Expert output projection rounded gated epilogue, retained

Reuse the existing cfg9 rounded gated epilogue for K=2048 output projection by passing K through its workspace/plan ABI. The FFN down path remains K=4096; no second kernel is added. All 50 rows of the K=2048 constant probes match exactly. All 180 actual calls pass the existing shallow tolerance (worst rel_rms 0.00194467); selected same-mainloop BF16 projection decompositions match exactly. Local reset-inclusive ABBA is 9.08560/7.40080/7.40196/9.08836 us per call. Its 18 weight sets total 72 MiB and can fit in the 96 MiB L2, so this isolated result alone did not establish deployment benefit.

Full-depth official comparison passed (action cosine 0.9999916982, rel_rms 0.00407475). Fresh-process end-to-end ABBA gives prior plan 33.139814/33.110785 ms and candidate 32.845290/32.833674 ms. The mean of medians improves by 0.285817 ms; A/B drift is 0.029029/0.011616 ms. The loaded plans differ only at action_expert_out_proj_residual. All four ending SM clocks are 2865 MHz, memory 13801 MHz and temperatures 57/57/56/57 C; the same power clock reason remains active. Other GPU work, compilation and model loading stayed paused during measurement. Source 7669007.

