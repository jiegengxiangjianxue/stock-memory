---
name: sources_registry_location
description: 项目数据源注册表路径与现有规模，调研数据源时需先读取以避免重复
type: workflow
---
项目 ~/workspace/stocks-ai-view 的数据源注册表位于 data/sources_registry.json，当前包含 83 个数据源。新增或调研数据源时，必须先读取该文件去重；同一厂商的新接口可视为新源，完全重复的排除。