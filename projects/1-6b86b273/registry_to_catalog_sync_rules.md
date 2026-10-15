---
name: registry_to_catalog_sync_rules
description: sources_registry 与 catalog JSON 的同步规则，避免覆盖已有证据
type: workflow
---
`scripts/merge_registry_to_catalog.py` 以 `sources_registry.py` 为权威源同步至 catalog JSON：
- catalog 中不存在的 id → 追加到映射 domain
- catalog 中已存在的 id → 保留 catalog 原条目，不覆盖（因探测证据更细）
- 状态映射：ok/degraded → available/degraded；blocked → blocked；unauth → unavailable；untested → unverified
运行方式：`python3 merge_registry_to_catalog.py`