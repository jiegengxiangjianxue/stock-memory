---
name: "analysis_manifest_freshness_source"
description: "前端判断各页面数据和算法是否新鲜时应读取统一分析发布清单。"
type: workflow
keywords: ["analysis", "manifest", "freshness", "source"]
---
每轮数据或引擎发布后运行 `scripts/build_analysis_manifest.py` 生成 `data/analysis_manifest.json`。清单记录交易日、当前会话、各页面数据文件的时间戳、年龄和状态、引擎成功情况及 stale 文件列表。前端应将其加入 `SOURCES`，作为判断各页面数据新鲜度和发布状态的唯一事实来源。