---
name: project_path_convention
description: stocks-ai-view 项目路径规范：适配脚本时的路径导入与输出落盘规则
type: workflow
---
stocks-ai-view 项目所有脚本必须从 `scripts/paths.py` 导入路径常量：`from paths import BULK, AI_VIEW_DATA, DUCKDB_PATH`。禁止硬编码 `/home/agentuser/stockdb/...` 或 `~/stockdb/...`。适配外部脚本时，输出统一落盘到 `AI_VIEW_DATA/data/*.json`。