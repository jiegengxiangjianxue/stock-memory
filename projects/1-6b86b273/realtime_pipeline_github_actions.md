---
name: realtime_pipeline_github_actions
description: 实时数据更新由 GitHub Actions 定时 workflow 驱动，数据经 GitHub raw 分发
type: workflow
---
实时数据链路依赖 GitHub Actions：realtime-minute 每 3 分钟（CST 09:27~11:32 / 13:00~15:02，UTC 1,2,3,5,6,7 时）、realtime-poll 每 5 分钟、full-pipeline 每 5 分钟（采集→合并去重→26 引擎计算→产出→push）。引擎代码唯一事实源是 scripts/ 目录。git push 到 GitHub CDN 有 1~5 分钟延迟，因此 cron 最细设为 3~5 分钟，不做 1 分钟级轮询。