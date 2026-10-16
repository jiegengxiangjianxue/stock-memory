---
name: "scripts_change_scope"
description: "处理 stocks-ai-view 的脚本任务时读取此约定，避免扩大改动范围或污染产物目录。"
type: workflow
---
仅修改 `scripts/flow_divergence.py` 和 `scripts/pool_audit.py`；不得修改其他文件、提交、推送或调用外网。验证时使用临时 fixture 或函数调用做语法与逻辑自验，不写入 `data/`。