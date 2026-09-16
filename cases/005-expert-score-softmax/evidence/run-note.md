来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 005 节；实验后记录，原文摘录。

## 005 — Expert masked attention, retained

Hypothesis: use tensor-core BF16 inputs with FP32 score output to remove Q/K casts and FP32 SIMT GEMM, then fuse scale/mask/softmax into one native kernel, preserving BF16 probabilities before the unchanged P@V GEMM. Synthetic real-shape tests include out=Q alias, runtime mask changes, and masked-V independence; worst output rel_rms 1.56e-4. Local graph medians 37.8 -> 15.2 us include identical Q reset copy on both sides. Full-depth real-weight official comparison passed.

Deployed shipped median 44.4914 -> 41.0824 ms, source 4d53ad7. This native chain replaces only expert attention; the rejected native-SDPA screen remains separately recorded.

