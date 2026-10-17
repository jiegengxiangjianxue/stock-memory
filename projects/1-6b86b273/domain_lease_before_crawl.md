---
name: "domain_lease_before_crawl"
description: "派发采集子任务前先占用域名租约，避免重复爬取和浪费 API 配额 [待确认 2026-10-17，与 domain_lease_required 冲突]"
type: workflow
pending: true
---
采集前必须用 `scripts/domain_lease.py` 租约：`--available` 查看，`--claim <agent> <domain>` 占用，完成后 `--release <agent> <domain> --status done --output <file> --note "<conclusion>"`。`done` 永久锁定该域；未释放的 claim 24 小时后自动过期。