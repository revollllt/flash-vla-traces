来源：`results/pi05-rtx5090/gpt6-run-01/README.md` 中的 001 节；实验后记录，原文摘录。

## 001 — Expert FFN pointwise fusion, retained

Hypothesis: remove 17 pointwise/cast launches per FFN call while retaining both torch.mm and all bf16 rounding points. Actual belt-cup invocation snapshots at seed42 gave exact outputs/factors for calls 0/17/90/179; local 180-call graph median 54.9115 -> 25.8879 us/call (5.224 ms sum-equivalent estimate, not a deployment measurement). Full-depth official parity passed after integration.

Deployed shipped median 59.5779 -> 55.4515 ms (-4.1264 ms, -6.93%). Raw reports 000/001 use matched conditions and independent processes. Observed clocks were 2872/2865 MHz and memory 13801 MHz, both ending at 52 C; candidate reports a software-power clock reason, so its smaller gain than the local estimate is not assigned solely to one cause. Large median separation versus within-run spread supports retaining it; no periodic control rerun was added. Source revision 23f3c8b.

