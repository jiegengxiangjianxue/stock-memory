---
name: data_sync_to_github_no_local
description: 数据存储红线：仅保留GitHub副本，本地不留数据
type: identity
---
用户明确约定：所有获取的数据必须同步至GitHub仓库，本地不保留任何数据副本。执行数据获取或同步任务时，应直接以GitHub仓库为目标，无需在本地落盘。