来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 007 节；实验后记录，原文摘录。

## 007 — CUTLASS backbone gate/up GEMMs, retained

The two large BF16 projections now use the tested128x128x64 Stream-K tile; native RMSNorm/GELU and BF16 projection output stay unchanged. The vendor revision matches the original screening library (main's cb4247394dd82148787aed73e5dc7cef33cbf862); a different installed CUTLASS checkout was detected and avoided. The two best screened tiles differed by less than the control drift, so one simpler cfg0 is used. Native compiler reports254registers and0spills; model capture/replay and all17realFFN calls passed.

Local FFN total11.591/11.862 ->10.866/10.883ms; full-depth official comparison passed. Deployed median37.9806 ->37.0332ms, source594e697. Only the gate/up call site is routed to CUTLASS in this trial; down is next.

