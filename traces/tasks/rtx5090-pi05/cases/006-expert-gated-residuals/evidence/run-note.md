来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 006 节；实验后记录，原文摘录。

## 006 — Expert gated residuals, retained

Hypothesis: each projection keeps the existing BF16 GEMM and fuses its seven cast/mul/add/store operations into one CUDA pass. Selected real inputs at calls0/17/90/179 for both sites match bitwise. Local timings include identical residual resets on both sides: output projection19.1919 ->7.7451us, FFN down22.8162 ->12.7092us, giving3.8797ms sum-equivalent potential. Scratch adds100KiB. Full-depth official comparison passed.

Deployed median41.0824 ->37.9806ms, source e98b74c. This comparison adds the same shared residual kernel at both expert projection sites; no GEMM or numerical tolerance changed.

The affected CPU Target declaration test initially found native library loading in the backbone wrapper factory. Loading now occurs only on first execution, allowing graph/route declarations without a GPU/compiler. CUDA arithmetic is unchanged; the scoped declaration/route checks pass (8+1 checks). This loader-only fix does not add a timing point.

