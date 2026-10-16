---
name: "domain_lease_required"
description: "派发或接手本项目子任务前，先按 AGENTS.md 管理领域租约。 [待确认 2026-10-17，与 stocks_ai_view_domain_lease 冲突]"
type: workflow
keywords: ["domain", "lease", "required", "派发或接手本项目子任务前", "先按", "AGENTS.md", "管理领域租约", "待确认"]
pending: true
---
本项目要求使用 `python3 scripts/domain_lease.py` 管理子代理领域：派发前检查可用领域并 claim，完成后 release；使用 `--status done` 释放会永久锁定该领域，租约通常 24 小时自动过期。