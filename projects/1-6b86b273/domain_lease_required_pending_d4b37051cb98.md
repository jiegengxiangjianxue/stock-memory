---
name: "domain_lease_required_pending_d4b37051cb98"
description: "派发子代理处理项目领域前后应先申请并在完成后释放领域租约。 [待确认 2026-10-17，与 domain_lease_required 冲突]"
type: workflow
pending: true
---
派发子代理前运行 `python3 scripts/domain_lease.py --available` 并用 `--claim <agent> <domain>` 认领领域；完成后用 `--release <agent> <domain> --status done --output <file> --note "<conclusion>"` 释放。`done` 会永久锁定该领域，认领会在 24 小时后自动过期。