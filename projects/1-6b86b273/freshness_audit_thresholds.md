---
name: "freshness_audit_thresholds"
description: "审计数据新鲜度时，按文件用途使用预设更新频率阈值。"
type: workflow
---
使用 `scripts/audit_freshness.py` 审计 `data/*.json`：realtime 类期望 15 分钟内更新，intraday 类 2 小时内更新，daily 类 26 小时内更新；超出对应阈值标记为 `STALE`，并生成 `data/freshness_audit.json` 与人读报告。