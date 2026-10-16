---
name: "domain_lease_required_pending_3082f4d284c6"
description: "派遣子代理处理项目领域前必须申请并在完成后释放领域租约。 [待确认 2026-10-17，与 domain_lease_required 冲突]"
type: workflow
pending: true
---
AGENTS.md 要求先运行 `python3 scripts/domain_lease.py --available` 查看可用领域，再用 `--claim <agent> <domain>` 认领；完成后用 `--release <agent> <domain> --status done --output <file> --note "<conclusion>"` 释放。`done` 会永久锁定该领域，认领会在 24 小时后自动过期。