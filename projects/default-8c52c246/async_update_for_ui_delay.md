---
name: async_update_for_ui_delay
description: 仅用于 UI 展示的延迟值应异步更新，避免同步 commit 阻塞主线程
type: fact
---
在更新 RuleStore 时，仅服务 UI 展示的数据（如节点测速 delay）必须使用异步 update，不要同步 commit() 整份配置，以免阻塞主线程。