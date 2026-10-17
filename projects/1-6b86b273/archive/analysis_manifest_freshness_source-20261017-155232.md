---
name: analysis_manifest_freshness_source
description: 前端判断各页面数据和算法是否新鲜时应读取统一分析发布清单。
type: workflow
---
每轮数据或引擎发布后运行 `scripts/build_analysis_manifest.py` 生成 `data/analysis_manifest.json`，记录交易日、会话、各页面文件时间戳与年龄、算法是否成功及 stale 文件列表。前端需将该文件加入 `SOURCES`，作为各页面数据新鲜度和发布状态的统一事实来源。