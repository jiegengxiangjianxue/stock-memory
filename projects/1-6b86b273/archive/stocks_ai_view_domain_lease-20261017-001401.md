---
name: "stocks_ai_view_domain_lease"
description: "在 stocks-ai-view 项目派发采集类子代理前，先检查并领取领域租约，完成后释放。 [待确认 2026-10-16，与 subagent_domain_lease 冲突]"
type: workflow
pending: true
---
项目 AGENTS.md 要求派发子代理前运行 `python3 scripts/domain_lease.py --available` 检查可用领域，再用 `--claim <agent> <domain>` 领取；结束后用 `--release <agent> <domain> --status done --output <file> --note "<conclusion>"` 释放。`done` 会永久锁定该领域，未完成的租约在 24 小时后自动过期；此机制用于避免重复抓取和浪费 API 配额。