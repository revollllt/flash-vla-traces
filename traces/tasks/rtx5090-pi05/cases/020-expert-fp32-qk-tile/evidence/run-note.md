来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 020 节；实验后记录，原文摘录。

## 020 — Expert FP32-score QK Triton tile, retained

The sole 32x32x64 tile replaces the QK GEMM. The runtime-mask native softmax, BF16 probability materialization, original torch PV/split-K reduction, output alias and scratch roles are retained. The current call remains one QK launch; this is a GEMM implementation change. All nine actual step/layer cases match bitwise for FP32 logits, BF16 probabilities and complete attention output. Reset-inclusive nine-call complete-chain ABBA gives control 118.314/118.315 us and candidate 110.126/110.133 us, about 0.91 us per call. Trace confirms the original later dispatches and the fixed QK kernel. Raw local evidence is in results/rtx5090-pi05/gpt6-attention-qk-triton.

Full-depth official comparison passed with action metrics equal to 019. Fresh-process end-to-end ABBA gives prior plan 32.406389/32.362382 ms and candidate 32.171719/32.211052 ms. The mean of medians improves by 0.193000 ms, with A/B drift 0.044007/0.039333 ms. Every candidate median remains below every control median; the observed separation ranges from approximately 0.151 to 0.235 ms. Only action_expert_attention differs.

Ending SM clocks are 2857/2865/2857/2865 MHz, memory 13801 MHz and temperatures 59/58/60/57 C. Both routes include one sample at each ending SM clock; these snapshots do not establish identical frequency histories. The gain is retained as deployment behavior under the existing policy without assigning the entire difference to isolated QK time. CPU wrapper declaration passed without CUDA initialization before integration. Source 76086d9.

A separate CPU-only layout hypothesis was dismissed: backbone QK and PV already use flat 2D GEMMs with Q/out(7744,256), K/V(968,256), and score/P(7744,968). There is no remaining batch dimension to remove by changing matmul to mm. No code, GPU screen or new softmax was introduced for that hypothesis.

