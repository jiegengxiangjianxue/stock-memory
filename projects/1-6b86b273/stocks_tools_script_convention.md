---
name: stocks_tools_script_convention
description: stocks 项目工具脚本的目录、输出路径与 JSON 字段规范
---
- **脚本目录**：`/root/workspace/stocks/tools-repo/tools/`，文件名模式 `{source}_stock_sentiment.py`（如 zhihu、tieba）。
- **数据输出**：`stocks-data-live/data/research/{source}_{YYYYMMDD}.json`；第二个可选参数可覆盖输出目录。
- **JSON 必含字段**：`timestamp`（ISO 8601）与 `source`（标识数据来源）。
- **加法式原则**：代码改动只增不删，适用于 schema 迁移与接口扩展。
- **架构约束**：多服务器 + GitHub 为单一可信源，数据/算法/接口/框架统一进 GitHub；本地任务只做探测与写入，不 git push、不碰远端。