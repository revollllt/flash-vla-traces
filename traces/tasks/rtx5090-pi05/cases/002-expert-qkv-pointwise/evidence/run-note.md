来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 002 节；实验后记录，原文摘录。

## 002 — Expert QKV pointwise fusion, retained

Two native kernels around the unchanged BF16 GEMM remove repeated casts and elementwise launches. Synthetic actual-shape tests at magnitudes 1, 1e-3 and 1e3 are bitwise equal including the factor, preserving KV-cache prefix sentinels; cold rotating graph samples are saved in the QKV candidate record. Full-depth real-weight official comparison passed after integration and its action metrics equal 001.

Deployed median 55.4515 -> 49.8801 ms, source c8f5e15. It is compared to the already-retained FFN version, not the initial model. The complete loaded plan is in measurements/002.json.

