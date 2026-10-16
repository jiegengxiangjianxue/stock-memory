---
name: daily_brief_update_schedule
description: 调整盘前日报或盘后分析生成时，遵循项目既定的北京时间定时安排。
type: workflow
---
`.github/workflows/daily-brief.yml` 按北京时间生成数据：每天 07:30 盘前、工作日 16:00 盘后、每天 20:00 盘后补充，周六 10:00 生成周度总结；该日报只在非交易时段生成。