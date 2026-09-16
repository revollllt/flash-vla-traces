# Flash-VLA Traces

[中文](README.md) | [English](README.en.md)

按任务保存优化中的证据、尝试与决策，供其他模型学习问题推进方式。

## 任务

| Task | 硬件 / 模型 | 教师 | 案例 |
|---|---|---|---|
| [rtx5090-pi05](traces/tasks/rtx5090-pi05/README.md) | RTX 5090 / Pi0.5 | GPT-6 Astra | 45 项索引，26 个双语案例；[中文索引](traces/tasks/rtx5090-pi05/index.md) · [English](traces/tasks/rtx5090-pi05/index.en.md) |

## 目录

```text
traces/
  tasks/
    rtx5090-pi05/
      README.md / README.en.md       # 任务条件、来源与边界
      index.md / index.en.md         # 本任务案例索引
      cases/
        001-expert-ffn-fusion/
          case.md / case.en.md      # 中英文六步案例
          trace.jsonl               # 原始可见事件摘录
          evidence/                 # 测量、补丁与来源说明
experiments/
  README.md / README.en.md           # 跨任务的学生实验使用约定
```

后续任务在 `traces/tasks/<task-id>/` 下各自保存说明、索引和案例，并加入上面的任务表。案例编号属于各自任务；跨任务引用使用 `rtx5090-pi05/001` 这样的完整标识。

各案例按“瓶颈 → 当时证据 → 假设 → 命令与改动 → 结果 → 保留/回退/继续调查”组织。中英文正文逐节对应；原始 trace、命令和证据共用原文。每个任务自行记录实际环境、代码版本、缺失信息和结论边界。

这是独立的本地 Git 仓库，暂未添加远程或 submodule。Flash-VLA 负责实现，本库负责案例；需要可执行复现时，再引入相应代码版本。学生实验的材料可见性和比较方式见 [使用约定](experiments/README.md)。
