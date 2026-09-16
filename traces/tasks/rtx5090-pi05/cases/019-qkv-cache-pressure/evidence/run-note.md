来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 019 节；实验后记录，原文摘录。

## 019 — Expert QKV Triton GEMM, retained

The fixed 16x32x32 Triton tile replaces only the middle GEMM; the existing native prepare, public factor, bias/RoPE/scatter and BF16 projected scratch stay in use. The two losing experimental tiles are not deployed. All 18 captured real layer calls match bitwise for projected output, Q/K/V and factor. PTX confirms BF16-input FP32-accumulator MMA followed by BF16 round-to-nearest before the store. The winner uses 40 registers, 6 KiB shared memory and no spills.

Because the actual 18 weights total 90 MiB and can fit L2, the pure-GEMM screen explicitly rotates two independent copies of the same real weights (180 MiB), with identical addresses/order for A and B. This is a cache-pressure experiment, not the deployment sequence. Its ABBA gives A9.9929/9.9529 us and B8.5680/8.5698 us. The subsequent original-18-weight full-chain ABBA gives A11.6107/11.1396 us and B9.9236/10.2329 us. The latter control/candidate drift is 0.4711/0.3093 us; its conservative separation is 0.9067 us/call, and the drift is retained in the report.

Full-depth official comparison passed with action metrics equal to 017. Fresh-process deployment ABBA gives prior plan 32.565772/32.616870 ms and candidate 32.426883/32.429453 ms. The mean of medians improves by 0.163153 ms; A/B drift is 0.051099/0.002570 ms. Only action_expert_norm_qkv_rope differs. All ending SM clocks are 2865 MHz, memory 13801 MHz and temperatures 59/59/57/58 C. The separated medians support retaining the gain, while the control drift limits exact attribution. CPU declaration/route checks passed 9/9 without CUDA initialization before integration. Source 0dcb021.

