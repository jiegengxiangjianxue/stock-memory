---
name: data_source_research_rules
description: 数据源调研任务的行为约束与输出文件要求
type: workflow
---
数据源调研仅做搜索与整理：不实测接口、不 pip install、不爬取、不改代码。只记录搜索结果明确出现的地址或官方文档地址，搜索不到写「未找到」；目标新增 20-40 个源；结果写入 /root/workspace/stocks-ai-view/docs/NEW_SOURCES_CANDIDATES.md，按 category 分组并标注最有价值源。