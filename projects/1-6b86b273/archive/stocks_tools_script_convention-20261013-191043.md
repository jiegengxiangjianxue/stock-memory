---
name: stocks_tools_script_convention
description: stocks 项目工具脚本的目录、输出路径与 JSON 字段规范
---
- **脚本目录**：`/root/workspace/stocks/tools-repo/tools/`。
- **数据输出**：`stocks-data-live/data/research/{source}_{YYYYMMDD}.json`（source 如 `zhihu`、`tieba`）。
- **JSON 必含字段**：`timestamp`（ISO 8601）和 `source`（标识数据来源）。
- **命名**：脚本名 `zhihu_stock_sentiment.py`、`tieba_stock_sentiment.py` 等，`{source}_stock_sentiment.py` 模式。
- 输出支持第二个可选参数覆盖输出目录。