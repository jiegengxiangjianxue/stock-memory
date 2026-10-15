---
name: data_on_git_no_local_copy
description: 数据统一提交到 Git，本地不保留副本，站点数据实时查询 GitHub/gitcode 数据库
---
项目数据统一提交到 Git 仓库，本地不保留数据副本；本地站的数据通过实时查询 GitHub/gitcode 的数据库获取，不在本地存储冗余数据。

## 用户会主动清理本地数据（2026-10-14 确认）

用户会随时手动删除本地数据目录（如 `~/workspace/stocks-db/data/` 整个消失）。
**这不是脚本 bug，也不是数据丢失**——云端 GitHub/GitCode 才是事实源。

遇到本地目录突然为空时：
1. 先确认云端完好（curl raw 端点验证 200），不要怀疑自己的脚本
2. 用 `stocks-db/scripts/restore_from_cloud.py` 拉回构建缓存
3. 不需要向用户报告「数据丢失」