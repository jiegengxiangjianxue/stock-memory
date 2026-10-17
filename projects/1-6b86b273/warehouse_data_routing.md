---
name: "warehouse_data_routing"
description: "排查仪表盘仓库数据来源或刷新延迟时，参考前端的数据读取顺序。 [待确认 2026-10-17，与 cloud_warehouse_authority 冲突]"
type: workflow
pending: true
---
`data_origin_patch.js` 的注释约定：仓库 JSON 优先从 `gh_sources.js` 配置的 A/B/C 仓 raw 地址中按 `ts` 选最新；失败后回退到 Pages 同源 `data/<file>`；仅 `market/*` 还有 GitCode 镜像兜底。A 是事实源，B/C 是错峰采集镜像；Pages 同源副本由 `pages-data-sync.yml` 从 A 同步，可能有约 10 分钟延迟。实时行情由 `stock-detail.js` 直接请求提供方，不经过此仓库 JSON 路由。