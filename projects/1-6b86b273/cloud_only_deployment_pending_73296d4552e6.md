---
name: "cloud_only_deployment_pending_73296d4552e6"
description: "进行本项目开发或审计时，先确认站点与数据访问遵循云端部署约定。 [待确认 2026-10-17，与 cloud_only_deployment 冲突]"
type: workflow
keywords: ["cloud", "only", "deployment", "pending", "73296d4552e6", "进行本项目开发或审计时", "先确认站点与数据访问遵循云端部署约定", "待确认"]
pending: true
---
本项目已退休本地数据库和本地网站，部署在云端；页面应直接读取云端 warehouse。不要依赖或重新引入本地 `data/` 目录或本地 DuckDB。