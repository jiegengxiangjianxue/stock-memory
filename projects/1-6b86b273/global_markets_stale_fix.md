---
name: "global_markets_stale_fix"
description: "读取全球股市数据异常或停留在旧快照时，先检查采集时段覆盖和专用工作流。 [待确认 2026-10-17，与 no_global_equity_futures 冲突]"
type: fact
keywords: ["global", "markets", "stale", "fix", "读取全球股市数据异常或停留在旧快照时", "先检查采集时段覆盖和专用工作流"]
pending: true
---
根因是 shard-collect 的 cron 仅覆盖 UTC 01:00–08:55（北京时间 09:00–16:55，主要是 A 股时段），导致欧美盘中/收盘及亚洲早盘期间 `data/global_markets.json` 保留旧快照。项目已增加 `.github/workflows/global-markets-realtime.yml`，在北京时间 16:00–17:00、21:30–23:59、00:00–02:00、07:00–08:55 等时段定时采集，并使用标准库调用腾讯、东财、新浪数据源；工作流 YAML 已验证，远端已更新至提交 `5be425a`。