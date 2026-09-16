来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 012 节；实验后记录，原文摘录。

## 012 — Expert down rounded gated epilogue, retained

The existing native CUTLASS library adds one cfg9 Stream-K broadcast epilogue. It reduces partial FP32 accumulators, rounds the GEMM result to BF16, then explicitly performs separate FP32 multiply and add before BF16 output. C=D preserves the in-place residual; no temporary projection is needed. All 180 actual calls passed existing shallow tolerance (worst rel_rms 0.00195104); selected same-mainloop BF16-projection decompositions match exactly. Local reset-inclusive ABBA is 12.72080/10.67627/10.68071/12.71182 us per call.

The first implementation failed numerical coverage and was not timed. Constant probes and comparison with the ordinary upstream epilogue isolated a missing destination fragment advance in the vendored broadcast reduce path. A Target-local adapter supplies that advance without changing the vendored header. All 50 rows of three constant probes then match exactly. The failed result remains recorded; see lab/pi05/rtx5090_expert_epilogue.md for the source evidence and fixed probe.

Full-depth official comparison passed (action cosine 0.9999849792, rel_rms 0.00548124). Four fresh-process A-B-B-A runs give prior plan 33.784778/33.769896 ms and candidate 33.499048/33.482750 ms. The mean of medians improves by 0.286438 ms; A/B drift is 0.014882/0.016298 ms. Only action_expert_ffn_down_residual differs between loaded plans. SM clocks ended 2865/2857/2865/2865 MHz, memory 13801 MHz, temperatures 58/57/58/58 C. Other compilation/model loading was paused during these end-to-end timings. Source df996b8.

