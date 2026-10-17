---
name: "freshness_audit_classifies_market_closure"
description: "排查数据未更新或被误报过时时，先区分正常休市、缺失时间戳和真正陈旧数据。"
type: fact
---
采集流程已加入 `scripts/audit_freshness.py` 新鲜度审计，并输出 `data/freshness_audit.json` 与 `data/freshness_audit.md`；审计会结合时间戳和交易时段区分预期市场休市与陈旧数据。审计超阈值时只在 Actions 日志发 warning，不阻断采集流程；相关修复提交为 `2ce8fdb` 和 `083a804`。