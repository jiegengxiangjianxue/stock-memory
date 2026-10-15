---
name: settings_dead_switch_audit
description: 设置项可切换但无效果的根因与审计方法
type: fact
---
本项目曾出现设置项可切换但无任何效果的问题，根因是 StorageManager 中声明的 Flow 在 SettingsViewModel 之外没有消费点（读取点）。审计方法：提取 StorageManager.kt 中所有 `val xxxFlow: Flow<...>`，再全项目搜索其 collect/first/value 读取点；零读取点即为死开关。新增设置时必须确认该 Flow 存在业务侧消费，否则视为未接入。