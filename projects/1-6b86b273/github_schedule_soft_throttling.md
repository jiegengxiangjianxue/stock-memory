---
name: "github_schedule_soft_throttling"
description: "排查 GitHub Actions 数据停更或工作流无故不触发时，优先检查仓库的 cron 触发点数量和调度限流。"
type: fact
---
仓库曾配置每小时 80+ 个 cron 触发点，导致 GitHub Actions schedule 出现静默停跑：`full-pipeline` 跑数轮后停约 2 小时，其他工作流也出现长时间不触发；当状态仍为 active 且没有 queued/in_progress 时，应考虑调度侧软限流，而非只排查 concurrency。项目采用单一每 5 分钟 `scheduler` cron，再通过 `workflow_dispatch` 级联唤醒下游作为修法。依据：`.github/workflows/scheduler.yml` 文件说明。