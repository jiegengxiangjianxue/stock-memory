---
name: global_markets_schedule_stale_fix
description: 全球市场数据曾因定时任务只覆盖有限时段而陈旧，需全天定时刷新。
type: fact
pending: true
---
根因是 `global-markets-realtime.yml` 原 cron 仅覆盖部分时段，漏掉美股、欧股夜盘和亚洲早盘，导致 `global_markets.json` 停在十几个小时前。修法是改为每 30 分钟运行一次：`0,30 0-18 * * *`，并保留 `workflow_dispatch` 作为手动兜底。