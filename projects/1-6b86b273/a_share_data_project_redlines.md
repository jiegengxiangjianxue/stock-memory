---
name: a_share_data_project_redlines
description: A股数据项目的架构红线与数据流约束，设计更新周期优化方案前必读
type: identity
---
- **项目红线**：本地容器不跑常驻服务；采集/算法只在云端跑（GitHub Actions + GitCode）。
- **数据流向**：采集 → GitHub 仓库（data/*.json）→ GitHub Pages 网站读 raw；git push 到 CDN 生效有 1-5 分钟延迟（已实测）。
- **当前调度**：GitHub Actions public 仓库，cron 每 5 分钟触发一次采集+轻量引擎链；GitHub Actions schedule 事件最小间隔 5 分钟是平台硬限制。
- **GitCode 现状**：流水线（CI）cron 最短 5 分钟，需申请内测名额；GitCode Space 有 30 分钟休眠，不适合常驻。
- **优化目标**：把数据更新周期从 5 分钟压到 2 分钟甚至更短；对象为全市场 5000+ 只股票行情 + 轻量引擎 + 网站可见。