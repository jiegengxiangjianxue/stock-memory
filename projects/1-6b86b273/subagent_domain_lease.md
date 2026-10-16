---
name: "subagent_domain_lease"
description: "派遣子代理抓取数据前，先按项目约定领取域名租约。 [待确认 2026-10-16，与 stocks_ai_view_project_rules 冲突]"
type: workflow
keywords: ["subagent", "domain", "lease", "派遣子代理抓取数据前", "先按项目约定领取域名租约", "待确认", "2026-10-16", "冲突"]
pending: true
---
项目 `stocks-ai-view` 的 `AGENTS.md` 要求派遣子代理前先领取 domain lease，完成后释放，避免重复抓取和浪费 API 配额。使用 `python3 scripts/domain_lease.py --available` 查看可用域名，`--claim <agent> <domain>` 领取，完成后用 `--release <agent> <domain> --status done --output <file> --note <conclusion>` 释放；`done` 会永久锁定该域名，领取记录 24 小时后自动过期。