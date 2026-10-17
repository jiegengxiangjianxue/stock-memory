---
name: "subagent_domain_lease_pending_9f2b7d902052"
description: "派发或执行项目子代理任务前后，遵循领域租约机制。 [待确认 2026-10-17，与 subagent_domain_lease 冲突]"
type: workflow
pending: true
---
项目要求：先用 `python3 scripts/domain_lease.py --available` 检查可用领域，使用 `--claim <agent> <domain>` 领取，完成后用 `--release <agent> <domain> --status done --output <file> --note "<conclusion>"` 释放；`done` 会永久锁定该领域，租约 24 小时自动过期。