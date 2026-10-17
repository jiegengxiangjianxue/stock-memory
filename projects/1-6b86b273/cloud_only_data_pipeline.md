---
name: "cloud_only_data_pipeline"
description: "进行股票数据采集、算法运行或引擎部署时，先遵守云端执行与静态数据发布约定。 [待确认 2026-10-17，与 cloud_only_stock_data 冲突]"
type: workflow
pending: true
---
本项目不在本地容器运行常驻服务；采集和算法仅在云端运行，使用 GitHub Actions 与 GitCode。数据流为：采集 → GitHub 仓库 `data/*.json` → GitHub Pages 读取 raw 数据；推送到 CDN 后网站生效通常有 1–5 分钟延迟。