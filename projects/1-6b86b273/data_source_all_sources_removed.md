---
name: data_source_all_sources_removed
description: SOURCES 数组为何排除 all_sources.json，以及替代溯源方式
type: fact
---
index.html 的 SOURCES 数组不加载 data/all_sources.json，因为 18.37MB 全量拉取会拖死首屏。需要全量溯源时直接查 GitHub raw，不要加回前端 SOURCES。