---
name: push2delay_rate_limit_boundary
description: 设计分钟级行情采集策略或遇到push2delay请求失败时参考
type: fact
---
实测push2delay接口30轮×60页、间隔60秒的连续请求：总1800页仅失败1次(0.06%)，首次失败发生在第28轮（约28分钟后），失败类型为URLError(5秒超时)。结论：分钟级采集方案可行，但需实现重试兜底机制。