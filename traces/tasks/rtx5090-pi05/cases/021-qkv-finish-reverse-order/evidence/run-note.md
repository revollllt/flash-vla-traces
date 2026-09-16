来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 021 节；实验后记录，原文摘录。

## 021 — Expert QKV rounded finish fusion, retained

The existing 16x32x32 Triton GEMM tile now performs the original factor multiplication, bias, adjacent-pair RoPE and output scatter after an explicit BF16 roundtrip. Native prepare and the public factor stay unchanged. K/V use the caller-provided suffix views without adding a second prefix offset. The 256,000-byte projected scratch and one finish launch are removed. All 18 actual-layer Q/K/V/factor outputs match bitwise and prefix caches stay identical. Compiled resources remain 40 registers, 6 KiB shared memory and no spills. Local complete-chain ABBA is A10.807111/B9.637333/B9.630222/A10.775111 us/call; its same-weight local cache conditions differ from deployment.

Full-depth official comparison passed with action metrics equal to 020. The scoped CPU Target check passes all 8 declarations and 1 route check. Fresh-process end-to-end ABBA gives A32.194986/B32.041923/B32.142267/A32.234348 ms. Mean-of-medians gain is 0.122573 ms, but B drift is 0.100344 ms versus A drift 0.039362 ms. Because candidate drift is close to the gain, one fixed reverse-order BAAB block was declared before collecting further results.

That reverse block gives B32.139037/A32.250799/A32.248522/B32.162340 ms. Its mean-of-medians gain is 0.098972 ms; A/B drift is 0.002277/0.023303 ms and minimum separation 0.086182 ms. All eight mean medians favor the candidate by 0.110772 ms. Candidate/control medians stay separated in both orders, supporting retention. The curve uses the first candidate measurement by the same convention as earlier iterations; the four candidate medians span 32.041923–32.162340 ms, so that first point alone is not the incremental gain estimate.

The eight ending SM clocks are 2857/2865/2857/2865/2857/2865/2857/2857 MHz, memory 13801 MHz, with temperatures 57/56/59/59/60/61/58/59 C. These snapshots do not establish identical clock histories. Only action_expert_norm_qkv_rope changes across the verified complete plan maps. No GPU work, compilation or model loading overlapped these end-to-end runs. Source 4de57c9; all reports and the predeclared follow-up decision are retained.

