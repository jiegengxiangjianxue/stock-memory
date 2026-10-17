---
name: "no_global_equity_futures"
description: "接入或补全球股市数据时先读 [待确认 2026-10-17，与 global_markets_stale_fix 冲突]"
type: fact
pending: true
---
用户明确说明：全球股市没有期货数据。做全球股市相关功能时不要请求、补全或依赖期货序列，缺数据也不能用期货代替。