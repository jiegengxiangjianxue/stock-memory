---
name: "cloud_only_stock_data"
description: "处理股票数据存储、同步或部署时，遵守项目的云端优先约定。 [待确认 2026-10-16，与 cloud_only_deployment 冲突]"
type: workflow
pending: true
---
本地数据库和本地网站已停用；网站从云端部署，直接读取云端数据仓库。获取的数据必须同步至 GitHub 仓库，本地不保留数据副本。