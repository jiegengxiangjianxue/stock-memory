---
name: cloud_only_no_local_deploy
description: 股票项目核心红线：采集/算法/引擎/调度只在云端（GitHub Actions + GitCode Space）运行，本地容器只写代码与只读调研，不部署常驻服务
type: identity
---
# 云端自动化红线（2026-10-14 用户明确要求）

**所有采集、算法、引擎、调度只跑在云端，不在本地容器部署。**

具体要求：
- 采集/引擎/复盘/预测的常驻逻辑放 GitHub Actions 或 GitCode Space，本地容器不作为运行环境
- 本地容器只用于：写代码、改仓库、推送、只读调研、一次性数据核对
- 不要在本地起常驻服务、cron、systemd、daemon
- 本地已有的大体量数据目录（stocks-db/data、stocks-ai-view/data/adjusted 263MB、stocks.duckdb）属于要清理的对象，不作为运行依赖

关联约束：
- 本地数据库要去除、本地网站也要去掉（降低容器占用）
- 依赖多个 GitCode Space 实现分析/算法/引擎，并覆盖 GitHub 上构建的网站
- 只在云端部署