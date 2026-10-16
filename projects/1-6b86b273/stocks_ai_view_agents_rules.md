---
name: "stocks_ai_view_agents_rules"
description: "维护 stocks-ai-view / stocks 系列仓库前必读：子代理域名租约与纯云端部署的强制约定 [待确认 2026-10-16，与 stocks_ai_view_project_rules 冲突]"
type: workflow
pending: true
---
项目 stocks-ai-view 的强制约定记录在 `AGENTS.md`：

1. **子代理域名租约（mandatory）**：派发子代理前必须先认领域名，完成后释放，避免重复抓取和浪费 API 配额。
```bash
python3 scripts/domain_lease.py --available
python3 scripts/domain_lease.py --claim <agent> <domain>
python3 scripts/domain_lease.py --release <agent> <domain> --status done \
  --output <file> --note "<conclusion>"
```
`done` 会永久锁定该域名；认领 24 小时后自动过期。

2. **纯云端部署（2026-10-14 起）**：本地数据库与本地网站均已退役；站点从云端部署，直接从云端仓库读数据。因此不再有本地 `data/` 目录，也不再使用本地 DuckDB。