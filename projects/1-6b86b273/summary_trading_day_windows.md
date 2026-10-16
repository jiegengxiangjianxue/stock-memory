---
name: "summary_trading_day_windows"
description: "实现 7 天、30 天、1 年总结时读取，以确定统计窗口口径。"
type: workflow
keywords: ["summary", "trading", "day", "windows", "实现", "30", "年总结时读取", "以确定统计窗口口径"]
---
实现主线统计或复盘归档时，读取已确认的交易日窗口口径：统计使用 7、30、365 个交易日窗口，而不是自然日；每日按同一交易日的盘前预测与盘后总结配对。输出需兼容旧的 `mainlines`、`daily`、`current` 字段，并新增 `windows` 字段，包含 7/30/365 各自的 `requested_days`、`observed_days`、`status` 及指标/主题；样本不足必须明确标注。