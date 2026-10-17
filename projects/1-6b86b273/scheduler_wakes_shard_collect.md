---
name: "scheduler_wakes_shard_collect"
description: "需要依赖定时心跳自动触发分片新闻采集时，读取该调度约定。"
type: workflow
---
`.github/workflows/scheduler.yml` 的默认 `WF` 列表必须包含 `shard-collect`，当前顺序为 `realtime-minute,shard-collect,full-pipeline,engine-chain,hot-impact-realtime,rebuild-pools,cloud-analysis,ai-update`。该修复已提交并推送，提交为 `f75b656`。