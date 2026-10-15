---
name: sources_registry_single_source
description: stocks-ai-view 项目数据源唯一事实来源与采集器架构
type: workflow
---
项目 stocks-ai-view 的所有数据源必须在 scripts/sources_registry.py 登记。采集器只读注册表、不硬编码 URL。新增源 = 注册表加一条记录 + 写一个 fetch 函数，无需改管道。注册字段：id, url, auth, status(ok/degraded/blocked/unauth/untested), freq, delay, fields, provides, notes, degrade_to。