来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 018 节；实验后记录，原文摘录。

## 018 — Single-launch expert PV, withdrawn after inconclusive deployment timing

The candidate used only the fixed 16x32x64 Triton PV tile, retaining the existing FP32 QK scores, runtime-mask softmax, materialized BF16 probabilities and out=Q alias. Nine actual step/layer pairs passed existing shallow tolerance (worst rel_rms 0.0027468, minimum cosine 0.999996229); results were not bitwise equal. The nine-pair warm-set ABBA totals were torch 50.786/51.024 us and Triton 43.946/43.915 us. Trace confirmed one PV launch and PTX used BF16-input FP32-accumulator MMA. Full-depth official comparison passed (action cosine 0.9999909392, rel_rms 0.00425716).

The first fresh-process ABBA gave A32.596582/B32.499377/B32.533664/A32.561475 ms: mean-of-medians difference 0.062508 ms, with A/B drift 0.035107/0.034287 ms and nearest separation only 0.027812 ms. Because the difference was close to the drift scale, one additional reverse-order BAAB block was specified before collecting more data; all eight runs are retained.

The fixed BAAB returned B32.489375/A32.557318/A32.638460/B32.602458 ms. It has a positive mean-of-medians difference of 0.051972 ms, but candidate/control medians overlap; A/B drift is 0.081142/0.113082 ms. Ending clocks change from 2865 MHz in B1/A1 to 2857 MHz in A2/B2, with temperatures 60/62/62/61 C. These snapshots cannot isolate the clock contribution. All eight mean medians favor the candidate by 0.057240 ms, but the reverse-order check does not provide stable separation under the existing deployment policy.

The candidate is recorded as inconclusive and its production module, registry entry and shipped route were removed in 1542c05. The original source is reviewable at 34add1b; lab experiments and all latency/correctness reports remain. The curve marks this measured trial as withdrawn and retains 017 as the deployed version. No more repetitions were used to seek a favorable result.

