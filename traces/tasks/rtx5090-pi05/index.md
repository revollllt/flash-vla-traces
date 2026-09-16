# RTX 5090 / Pi0.5 案例索引

[全部任务](../../../README.md) · [任务说明](README.md) · [中文](index.md) | [English](index.en.md)

共 45 个案例单位，**26 个已整理为中英文六步案例**，其余 19 个仅列索引。本次新增 14 个案例，已覆盖除 023/024 外的全部 20 个保留优化；原有 023 保留并提供翻译，024 按本次范围不展开。

24 个部署试验中，22 个保留、2 个因部署收益不确定而撤回；19 个未部署案例包含 14 个局部计时案例、4 个 CPU 筛选案例和 1 个实现失败案例；另有 2 个诊断案例。041 把 BM32/BM16 合为一个假设演进案例，012 的修复归入同一优化案例，040 的 out-projection 正结果归入 024。

这些案例来自同一次累积优化，存在前后依赖和共享证据，**不是 45 个独立实验样本**。中英文共用原始 trace 与证据，翻译不替代原文。

| ID | 案例（中文） | English | 类型 | 结果 | 整理状态 |
|---|---|---|---|---|---|
| 001 | [Expert FFN 逐元素融合](cases/001-expert-ffn-fusion/case.md) | [English](cases/001-expert-ffn-fusion/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 002 | [Expert QKV norm/bias/RoPE 融合](cases/002-expert-qkv-pointwise/case.md) | [English](cases/002-expert-qkv-pointwise/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 003 | [Backbone FFN norm/activation 融合](cases/003-backbone-ffn-pointwise/case.md) | [English](cases/003-backbone-ffn-pointwise/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 004 | [Expert gate/up 打包 GEMM](cases/004-packed-expert-ffn/case.md) | [English](cases/004-packed-expert-ffn/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 005 | [Expert BF16 QK / FP32 scores 与 masked softmax](cases/005-expert-score-softmax/case.md) | [English](cases/005-expert-score-softmax/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 006 | [Expert 投影 gated residual 融合](cases/006-expert-gated-residuals/case.md) | [English](cases/006-expert-gated-residuals/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 007 | [Backbone gate/up CUTLASS Stream-K](cases/007-backbone-cutlass-gate-up/case.md) | [English](cases/007-backbone-cutlass-gate-up/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 008 | [Backbone down CUTLASS](cases/008-backbone-cutlass-down/case.md) | [English](cases/008-backbone-cutlass-down/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 009 | [Vision LayerNorm/GELU 融合](cases/009-vision-normalization-activation/case.md) | [English](cases/009-vision-normalization-activation/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 010 | [Prefix QKV RMSNorm/RoPE scatter](cases/010-prefix-qkv-pointwise/case.md) | [English](cases/010-prefix-qkv-pointwise/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 011 | [Action 输出归一化与 Euler 更新融合](cases/011-action-output-pointwise/case.md) | [English](cases/011-action-output-pointwise/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 012 | [Expert down rounded gated epilogue 与覆盖修复](cases/012-rounded-epilogue-coverage/case.md) | [English](cases/012-rounded-epilogue-coverage/case.en.md) | 部署试验 | 修复后保留 | 双语正文、trace、证据已保存 |
| 013 | [Vision FFN 两投影统一 cfg10](cases/013-vision-ffn-shared-tile/case.md) | [English](cases/013-vision-ffn-shared-tile/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 014 | [Expert out projection 复用 rounded epilogue](cases/014-expert-output-rounded-epilogue/case.md) | [English](cases/014-expert-output-rounded-epilogue/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 015 | [Vision QKV 复用 cfg10](cases/015-vision-qkv-cfg10/case.md) | [English](cases/015-vision-qkv-cfg10/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 016 | [Expert FFN dual-dot 与 rounded GELU](cases/016-dual-dot-ffn/case.md) | [English](cases/016-dual-dot-ffn/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 017 | [Vision out projection 复用 cfg10](cases/017-vision-output-cfg10/case.md) | [English](cases/017-vision-output-cfg10/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 018 | [Expert PV 单 launch Triton](cases/018-pv-inconclusive-revert/case.md) | [English](cases/018-pv-inconclusive-revert/case.en.md) | 部署试验 | 不确定，撤回 | 双语正文、trace、证据已保存 |
| 019 | [Expert QKV 固定 Triton GEMM 与缓存压力验证](cases/019-qkv-cache-pressure/case.md) | [English](cases/019-qkv-cache-pressure/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 020 | [Expert QK 固定 Triton FP32-score tile](cases/020-expert-fp32-qk-tile/case.md) | [English](cases/020-expert-fp32-qk-tile/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 021 | [Expert QKV GEMM finish 融合与反向复测](cases/021-qkv-finish-reverse-order/case.md) | [English](cases/021-qkv-finish-reverse-order/case.en.md) | 部署试验 | 保留 | 双语正文、trace、证据已保存 |
| 022 | Vision rounded GELU epilogue | — | 部署试验 | 不确定，撤回 | 仅索引，正文待整理 |
| 023 | [Runtime 896/968 backbone FFN 分桶](cases/023-runtime-prefix-buckets/case.md) | [English](cases/023-runtime-prefix-buckets/case.en.md) | 部署试验 | 有条件保留 | 原有案例保留，已翻译；不计本次新增 |
| 024 | Runtime 896/968 backbone out projection 分桶 | — | 部署试验 | 有条件保留 | 本次范围排除，未展开 |
| 025 | Native SDPA dispatch 筛选 | — | 未部署的筛选/试验 | 已测路径更慢/部分不支持 | 仅索引，正文待整理 |
| 026 | [Vision residual 独立逐元素融合](cases/026-vision-residual-source-screen/case.md) | [English](cases/026-vision-residual-source-screen/case.en.md) | 未部署的筛选/试验 | CPU 筛选停止 | 双语正文、trace、证据已保存 |
| 027 | PV K1018 补齐 K1024 | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 028 | Backbone out projection 直接复用 cfg0 | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 029 | Backbone up GEMM 融合 GELU/product epilogue | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 030 | Expert down 固定 Triton 16×32×32 | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 031 | Expert QK/softmax full-row CTA 可行性 | — | 未部署的筛选/试验 | CPU 筛选停止；转置布局未测 | 仅索引，正文待整理 |
| 032 | Expert softmax 128 vs 256 threads | — | 未部署的筛选/试验 | 差值未超过漂移，停止 | 仅索引，正文待整理 |
| 033 | CUTLASS expert down M16 vs M32 | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 034 | Prefix QKV 确切形状复用 cfg0 | — | 未部署的筛选/试验 | 差值未超过漂移，停止 | 仅索引，正文待整理 |
| 035 | Expert down warpN16/count4 联合布局 | — | 未部署的筛选/试验 | 实现失败；无性能结论 | 仅索引，正文待整理 |
| 036 | Dual FFN 4→8 warps | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 037 | Expert down 显式 split8 调度 | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 038 | Expert softmax/PV CuTe 融合 | — | 未部署的筛选/试验 | 局部更慢，拒绝 | 仅索引，正文待整理 |
| 039 | 双 bucket launch 合为单一最大 grid | — | 未部署的筛选/试验 | CPU 可行性分析后暂缓 | 仅索引，正文待整理 |
| 040 | Prefix QKV 静态 M896 | — | 未部署的筛选/试验 | 静态收益不足，停止 | 仅索引，正文待整理 |
| 041 | [Backbone full-row attention BM32→BM16](cases/041-fullrow-attention-rejections/case.md) | [English](cases/041-fullrow-attention-rejections/case.en.md) | 未部署的筛选/试验 | 两个局部映射均拒绝 | 双语正文、trace、证据已保存 |
| 042 | Backbone M896 cfg1 可行性 | — | 未部署的筛选/试验 | CPU 分析后暂缓；确切形状未测 | 仅索引，正文待整理 |
| 043 | RMS prepare 合入 dual FFN | — | 未部署的筛选/试验 | 逐位一致但局部更慢，拒绝 | 仅索引，正文待整理 |
| 044 | [CUTLASS 双动态库 GNU-unique TLS 冲突](cases/044-cutlass-shared-tls/case.md) | [English](cases/044-cutlass-shared-tls/case.en.md) | 诊断 | 定位故障，修正探针 | 双语正文、trace、证据已保存 |
| 045 | [NCU Tensor/SOL 与低 occupancy 的解释](cases/045-ncu-low-occupancy/case.md) | [English](cases/045-ncu-low-occupancy/case.en.md) | 诊断 | 调整优化优先级 | 双语正文、trace、证据已保存 |
