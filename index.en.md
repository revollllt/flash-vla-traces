# Case index

[中文](index.md) | [English](index.en.md)

There are 45 case units: **26 have corresponding Chinese and English six-stage narratives**, and 19 remain index entries. This update adds 14 cases, covering all 20 retained optimizations other than 023/024. Existing case 023 is preserved and translated; 024 is outside this expansion.

Of 24 deployment trials, 22 were retained and 2 withdrawn for inconclusive deployment gains. The 19 non-deployment cases comprise 14 locally timed case units, 4 CPU screening cases and 1 implementation failure; another 2 cases concern diagnostics. Case 041 groups BM32/BM16 as one evolving hypothesis; 012 includes its repair; the positive output-projection result from 040 belongs to 024.

These cases come from one cumulative optimization run, with dependencies and shared evidence. **They are not 45 independent experimental samples.** Both languages share original traces and evidence; translations do not replace source records.

| ID | Case (English) | 中文 | Type | Outcome | Documentation status |
|---|---|---|---|---|---|
| 001 | [Expert FFN pointwise fusion](cases/001-expert-ffn-fusion/case.en.md) | [中文](cases/001-expert-ffn-fusion/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 002 | [Expert QKV norm/bias/RoPE fusion](cases/002-expert-qkv-pointwise/case.en.md) | [中文](cases/002-expert-qkv-pointwise/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 003 | [Backbone FFN norm/activation fusion](cases/003-backbone-ffn-pointwise/case.en.md) | [中文](cases/003-backbone-ffn-pointwise/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 004 | [Packed Expert gate/up GEMM](cases/004-packed-expert-ffn/case.en.md) | [中文](cases/004-packed-expert-ffn/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 005 | [Expert BF16 QK / FP32 scores and masked softmax](cases/005-expert-score-softmax/case.en.md) | [中文](cases/005-expert-score-softmax/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 006 | [Expert projection gated-residual fusion](cases/006-expert-gated-residuals/case.en.md) | [中文](cases/006-expert-gated-residuals/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 007 | [Backbone gate/up CUTLASS Stream-K](cases/007-backbone-cutlass-gate-up/case.en.md) | [中文](cases/007-backbone-cutlass-gate-up/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 008 | [Backbone down CUTLASS](cases/008-backbone-cutlass-down/case.en.md) | [中文](cases/008-backbone-cutlass-down/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 009 | [Vision LayerNorm/GELU fusion](cases/009-vision-normalization-activation/case.en.md) | [中文](cases/009-vision-normalization-activation/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 010 | [Prefix QKV RMSNorm/RoPE scatter](cases/010-prefix-qkv-pointwise/case.en.md) | [中文](cases/010-prefix-qkv-pointwise/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 011 | [Action-output normalization and Euler-update fusion](cases/011-action-output-pointwise/case.en.md) | [中文](cases/011-action-output-pointwise/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 012 | [Expert down rounded gated epilogue and coverage repair](cases/012-rounded-epilogue-coverage/case.en.md) | [中文](cases/012-rounded-epilogue-coverage/case.md) | Deployment trial | Retained after repair | Bilingual narrative, trace and evidence saved |
| 013 | [One cfg10 for both Vision FFN projections](cases/013-vision-ffn-shared-tile/case.en.md) | [中文](cases/013-vision-ffn-shared-tile/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 014 | [Rounded-epilogue reuse for Expert output projection](cases/014-expert-output-rounded-epilogue/case.en.md) | [中文](cases/014-expert-output-rounded-epilogue/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 015 | [Vision QKV cfg10 reuse](cases/015-vision-qkv-cfg10/case.en.md) | [中文](cases/015-vision-qkv-cfg10/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 016 | [Expert FFN dual-dot and rounded GELU](cases/016-dual-dot-ffn/case.en.md) | [中文](cases/016-dual-dot-ffn/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 017 | [Vision output-projection cfg10 reuse](cases/017-vision-output-cfg10/case.en.md) | [中文](cases/017-vision-output-cfg10/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 018 | [Single-launch Expert PV Triton](cases/018-pv-inconclusive-revert/case.en.md) | [中文](cases/018-pv-inconclusive-revert/case.md) | Deployment trial | Inconclusive; withdrawn | Bilingual narrative, trace and evidence saved |
| 019 | [Fixed Expert QKV Triton GEMM and cache-pressure check](cases/019-qkv-cache-pressure/case.en.md) | [中文](cases/019-qkv-cache-pressure/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 020 | [Fixed Expert QK FP32-score Triton tile](cases/020-expert-fp32-qk-tile/case.en.md) | [中文](cases/020-expert-fp32-qk-tile/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 021 | [Expert QKV GEMM finish fusion and reverse-order check](cases/021-qkv-finish-reverse-order/case.en.md) | [中文](cases/021-qkv-finish-reverse-order/case.md) | Deployment trial | Retained | Bilingual narrative, trace and evidence saved |
| 022 | Vision rounded GELU epilogue | — | Deployment trial | Inconclusive; withdrawn | Index only; narrative pending |
| 023 | [Runtime 896/968 backbone FFN buckets](cases/023-runtime-prefix-buckets/case.en.md) | [中文](cases/023-runtime-prefix-buckets/case.md) | Deployment trial | Conditionally retained | Existing case preserved and translated; not a new addition |
| 024 | Runtime 896/968 backbone output-projection buckets | — | Deployment trial | Conditionally retained | Outside this expansion; index only |
| 025 | Native SDPA dispatch screening | — | Non-deployment screen/trial | Tested routes slower / some unsupported | Index only; narrative pending |
| 026 | [Standalone Vision residual pointwise fusion](cases/026-vision-residual-source-screen/case.en.md) | [中文](cases/026-vision-residual-source-screen/case.md) | Non-deployment screen/trial | Stopped at CPU screening | Bilingual narrative, trace and evidence saved |
| 027 | Pad PV K1018 to K1024 | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 028 | Direct cfg0 reuse for backbone output projection | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 029 | Backbone up GEMM GELU/product epilogue | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 030 | Fixed 16×32×32 Triton Expert down | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 031 | Expert QK/softmax full-row CTA feasibility | — | Non-deployment screen/trial | Stopped at CPU screening; transposed layout untested | Index only; narrative pending |
| 032 | Expert softmax 128 versus 256 threads | — | Non-deployment screen/trial | Difference did not exceed drift; stopped | Index only; narrative pending |
| 033 | CUTLASS Expert down M16 versus M32 | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 034 | Exact-shape prefix QKV cfg0 reuse | — | Non-deployment screen/trial | Difference did not exceed drift; stopped | Index only; narrative pending |
| 035 | Expert down joint warpN16/count4 layout | — | Non-deployment screen/trial | Implementation failure; no performance conclusion | Index only; narrative pending |
| 036 | Dual FFN 4→8 warps | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 037 | Expert down explicit split8 scheduling | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 038 | Expert softmax/PV CuTe fusion | — | Non-deployment screen/trial | Locally slower; rejected | Index only; narrative pending |
| 039 | Replace two bucket launches with one maximum grid | — | Non-deployment screen/trial | Deferred after CPU feasibility review | Index only; narrative pending |
| 040 | Static M896 prefix QKV | — | Non-deployment screen/trial | Insufficient static benefit; stopped | Index only; narrative pending |
| 041 | [Backbone full-row attention BM32→BM16](cases/041-fullrow-attention-rejections/case.en.md) | [中文](cases/041-fullrow-attention-rejections/case.md) | Non-deployment screen/trial | Both local mappings rejected | Bilingual narrative, trace and evidence saved |
| 042 | Backbone M896 cfg1 feasibility | — | Non-deployment screen/trial | Deferred after CPU review; exact shape unmeasured | Index only; narrative pending |
| 043 | RMS prepare fused into dual FFN | — | Non-deployment screen/trial | Bitwise equal but locally slower; rejected | Index only; narrative pending |
| 044 | [GNU-unique TLS collision between CUTLASS libraries](cases/044-cutlass-shared-tls/case.en.md) | [中文](cases/044-cutlass-shared-tls/case.md) | Diagnostic | Fault isolated; probe corrected | Bilingual narrative, trace and evidence saved |
| 045 | [NCU Tensor/SOL interpretation of low occupancy](cases/045-ncu-low-occupancy/case.en.md) | [中文](cases/045-ncu-low-occupancy/case.md) | Diagnostic | Optimization priorities revised | Bilingual narrative, trace and evidence saved |
