---
name: "stocks_ai_view_project_rules"
description: "在 stocks-ai-view 项目里派发子代理抓取或改动架构前必读：域名租约与纯云端部署两条硬规则 [待确认 2026-10-16，与 cloud_only_deployment 冲突]"
type: workflow
pending: true
---
项目根 `AGENTS.md` 规定两条强制约定：

1. **子代理域名租约（强制）**：派发抓取任务前先认领，避免重复爬取与 API 配额浪费。命令：
```bash
python3 scripts/domain_lease.py --available
python3 scripts/domain_lease.py --claim <agent> <domain>
python3 scripts/domain_lease.py --release <agent> <domain> --status done \
  --output <file> --note "<conclusion>"
```
`done` 会永久锁定该域名，未完成的认领 24 小时后自动过期。

2. **纯云端部署（2026-10-14 起）**：本地数据库与本地网站均已废弃，站点评由云端部署、数据直接读云仓库。不再有本地 `data/` 目录，不再使用本地 DuckDB，所有读取都走云端。