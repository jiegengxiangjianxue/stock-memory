---
name: "github_actions_schedule_limit"
description: "规划实时更新频率和验证网站数据时，考虑平台调度与发布延迟，避免把不可达频率当作实现目标。"
type: fact
---
当前公开 GitHub Actions 的采集与轻量引擎链按每 5 分钟运行一次；GitHub Actions 的 schedule 最小间隔已确认是 5 分钟的平台限制。GitCode 流水线 cron 最短也是 5 分钟且需申请内测名额，GitCode Space 会休眠 30 分钟，不适合常驻服务。将数据更新从 5 分钟压到 2 分钟或更短仍是优化目标，但不能仅依赖现有 schedule 配置。