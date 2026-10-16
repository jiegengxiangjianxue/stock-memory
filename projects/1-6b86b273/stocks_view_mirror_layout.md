---
name: "stocks_view_mirror_layout"
description: "改动 stocks-ai-view 站点前端、多源数据镜像或数据源配置时的站点与仓库约定"
type: fact
---
- 站点形态：单站点布局；数据源中 C 角色只承担采集，不参与站点渲染（见提交 docs(mirror): record the single-site layout and C's collection-only role）。
- 前端聚合：index.html 引用 data_origin_patch.js 与 gh_sources.js；gh_sources.js 内的 SOURCES 记录多源仓库，形如 { owner: "qwerzxcva", repo: "stocks-ai-view", branch: "main", label: "A" } 与 { owner: "jiegengxiangjianxue", repo: "stocks-ai-view-b", branch: "main", ... }（label A/B 等）。
- data_origin_patch.js 内含 fetchFreshest、multiUrls、fetchJSONMinute 等取数逻辑。
- 未跟踪文件 data/xq_screener.json、data/xq_screener_full.json 存在于数据目录中。