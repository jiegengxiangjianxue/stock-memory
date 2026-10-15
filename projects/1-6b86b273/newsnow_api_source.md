---
name: newsnow_api_source
description: 免 key 的 Newsnow 公共 API 热点数据源接入方案
type: fact
---
Newsnow 公共 API 可直接获取热点数据，无需 key：`GET https://newsnow.busiyi.world/api/s?id=<source>`。实测 15/17 个源可用（`linuxdo-hot` 返回 HTTPError，其余实时更新），覆盖雪球、财联社电报、华尔街见闻、知乎、微博、B站、抖音、头条等。脚本实现：`scripts/collect_hotnews.py`，支持 `--sources` 指定源、`--out` 输出路径（默认 `data/hot_news.json`）；借鉴 TrendRadar 实现频率词分组匹配（每个标题只计入第一个匹配词组）、`!` 开头过滤词丢弃、保留 `rank` 热度字段。