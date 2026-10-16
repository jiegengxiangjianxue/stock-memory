---
name: pool_freshness_audit_ui
description: 修改股票池展示或数据接入时，需保留多池时间戳与过期状态提示。
type: workflow
---
`index.html` 的 `renderPools()` 从 `window._data.pool_audit.pools` 读取各池状态；任何 `status` 非 `fresh` 的池都应显示为“部分过期”，并展示池名及 `age_hours`，否则显示“正常/所有池均有时间戳”。