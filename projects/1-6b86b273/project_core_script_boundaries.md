---
name: project_core_script_boundaries
description: stocks-ai-view 项目核心脚本红线与提交禁令
type: identity
---
stocks-ai-view 项目中，助手不得修改以下核心脚本：`paths.py`、`decision_engine.py`、`engine_v5.py`、`self_reflect.py`、`ai_screener.py`、`collect_all_sources.py`。所有 git commit / push 操作必须由用户手动执行，助手不得代为提交。环境缺库时不安装。