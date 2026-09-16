# 案例索引

共 45 个案例单位。首批 12 个已整理，其余 33 个保留清单，不冒充完成的教学案例。

24 个部署试验中，22 个保留、2 个因部署收益不确定而撤回；19 个未部署案例包含 14 个局部计时案例、4 个 CPU 筛选案例和 1 个实现失败案例；另有 2 个诊断案例。041 把 BM32/BM16 合为一个假设演进案例，012 的修复归入同一优化案例，040 的 out-projection 正结果归入 024。

这些案例来自同一次累积优化，存在前后依赖和共享证据，**不是 45 个独立实验样本**。

| ID | 案例 | 类型 | 结果 | 整理状态 |
|---|---|---|---|---|
| 001 | [Expert FFN 逐元素融合](cases/001-expert-ffn-fusion/case.md) | 部署试验 | 保留 | 已整理六步案例、trace、证据 |
| 002 | Expert QKV norm/bias/RoPE 融合 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 003 | Backbone FFN norm/activation 融合 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 004 | Expert gate/up 打包 GEMM | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 005 | Expert BF16 QK / FP32 scores 与 masked softmax | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 006 | Expert 投影 gated residual 融合 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 007 | [Backbone gate/up CUTLASS Stream-K](cases/007-backbone-cutlass-gate-up/case.md) | 部署试验 | 保留 | 已整理六步案例、trace、证据 |
| 008 | Backbone down CUTLASS | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 009 | Vision LayerNorm/GELU 融合 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 010 | Prefix QKV RMSNorm/RoPE scatter | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 011 | Action 输出归一化与 Euler 更新融合 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 012 | [Expert down rounded gated epilogue 与覆盖修复](cases/012-rounded-epilogue-coverage/case.md) | 部署试验 | 修复后保留 | 已整理六步案例、trace、证据 |
| 013 | Vision FFN 两投影统一 cfg10 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 014 | Expert out projection 复用 rounded epilogue | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 015 | Vision QKV 复用 cfg10 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 016 | [Expert FFN dual-dot 与 rounded GELU](cases/016-dual-dot-ffn/case.md) | 部署试验 | 保留 | 已整理六步案例、trace、证据 |
| 017 | Vision out projection 复用 cfg10 | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 018 | [Expert PV 单 launch Triton](cases/018-pv-inconclusive-revert/case.md) | 部署试验 | 不确定，撤回 | 已整理六步案例、trace、证据 |
| 019 | [Expert QKV 固定 Triton GEMM 与缓存压力验证](cases/019-qkv-cache-pressure/case.md) | 部署试验 | 保留 | 已整理六步案例、trace、证据 |
| 020 | Expert QK 固定 Triton FP32-score tile | 部署试验 | 保留 | 已列入索引，正文待整理 |
| 021 | [Expert QKV GEMM finish 融合与反向复测](cases/021-qkv-finish-reverse-order/case.md) | 部署试验 | 保留 | 已整理六步案例、trace、证据 |
| 022 | Vision rounded GELU epilogue | 部署试验 | 不确定，撤回 | 已列入索引，正文待整理 |
| 023 | [Runtime 896/968 backbone FFN 分桶](cases/023-runtime-prefix-buckets/case.md) | 部署试验 | 有条件保留 | 已整理六步案例、trace、证据 |
| 024 | Runtime 896/968 backbone out projection 分桶 | 部署试验 | 有条件保留 | 已列入索引，正文待整理 |
| 025 | Native SDPA dispatch 筛选 | 未部署的筛选/试验 | 已测路径更慢/部分不支持 | 已列入索引，正文待整理 |
| 026 | [Vision residual 独立逐元素融合](cases/026-vision-residual-source-screen/case.md) | 未部署的筛选/试验 | CPU 筛选停止 | 已整理六步案例、trace、证据 |
| 027 | PV K1018 补齐 K1024 | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 028 | Backbone out projection 直接复用 cfg0 | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 029 | Backbone up GEMM 融合 GELU/product epilogue | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 030 | Expert down 固定 Triton 16×32×32 | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 031 | Expert QK/softmax full-row CTA 可行性 | 未部署的筛选/试验 | CPU 筛选停止；转置布局未测 | 已列入索引，正文待整理 |
| 032 | Expert softmax 128 vs 256 threads | 未部署的筛选/试验 | 差值未超过漂移，停止 | 已列入索引，正文待整理 |
| 033 | CUTLASS expert down M16 vs M32 | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 034 | Prefix QKV 确切形状复用 cfg0 | 未部署的筛选/试验 | 差值未超过漂移，停止 | 已列入索引，正文待整理 |
| 035 | Expert down warpN16/count4 联合布局 | 未部署的筛选/试验 | 实现失败；无性能结论 | 已列入索引，正文待整理 |
| 036 | Dual FFN 4→8 warps | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 037 | Expert down 显式 split8 调度 | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 038 | Expert softmax/PV CuTe 融合 | 未部署的筛选/试验 | 局部更慢，拒绝 | 已列入索引，正文待整理 |
| 039 | 双 bucket launch 合为单一最大 grid | 未部署的筛选/试验 | CPU 可行性分析后暂缓 | 已列入索引，正文待整理 |
| 040 | Prefix QKV 静态 M896 | 未部署的筛选/试验 | 静态收益不足，停止 | 已列入索引，正文待整理 |
| 041 | [Backbone full-row attention BM32→BM16](cases/041-fullrow-attention-rejections/case.md) | 未部署的筛选/试验 | 两个局部映射均拒绝 | 已整理六步案例、trace、证据 |
| 042 | Backbone M896 cfg1 可行性 | 未部署的筛选/试验 | CPU 分析后暂缓；确切形状未测 | 已列入索引，正文待整理 |
| 043 | RMS prepare 合入 dual FFN | 未部署的筛选/试验 | 逐位一致但局部更慢，拒绝 | 已列入索引，正文待整理 |
| 044 | [CUTLASS 双动态库 GNU-unique TLS 冲突](cases/044-cutlass-shared-tls/case.md) | 诊断 | 定位故障，修正探针 | 已整理六步案例、trace、证据 |
| 045 | [NCU Tensor/SOL 与低 occupancy 的解释](cases/045-ncu-low-occupancy/case.md) | 诊断 | 调整优化优先级 | 已整理六步案例、trace、证据 |
