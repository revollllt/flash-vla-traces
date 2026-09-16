来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 003 节；实验后记录，原文摘录。

## 003 — Backbone FFN pointwise fusion, retained

Hypothesis: remove full FP32 cast/intermediate traversals around the two unchanged large GEMMs. Seventeen real call snapshots passed existing shallow tolerance: worst output rel_rms 1.14e-4 and minimum cosine 0.9999999935. Local graph totals torch 15.698/15.975 ms vs fused 11.936/11.971 ms, with reference drift noted. Full-depth official comparison passed after deployment.

Deployed median 49.8801 -> 45.8682 ms, source 571b9b4. Raw local/official/end-to-end evidence is saved alongside this trial. The change affects prefix KV numerics within existing tolerances, not the model precision policy.

