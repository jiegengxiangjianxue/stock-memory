---
name: "daily_mainline_and_summaries"
description: "改主线或实时总结时读：每日主线命名、日期表，以及 7/30/365 总结窗口"
type: workflow
---
主线与总结的产品约定：
- 「今日主线」改名为「每日主线」，必须标日期与时间；顶部那条今日主线移进近期主线，不要单独挂在最上面。
- 近期主线用日期表，每天一行对应该日主线；每日开盘预测和收盘后总结都要展示（比现在多两份）。
- 实时总结要加近 7 天 / 30 天 / 一年三个窗口，分析要更严谨、提高信息净度，不能只堆表面结论。
入口：renderDailySummary / renderMainline；近期窗口在 g.recent_mainlines.windows，DOM 为 #summaryWindowsBox。