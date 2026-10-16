---
name: "restricted_review_script_changes"
description: "修改该项目复盘统计时，遵守文件范围、数据保护与验证要求。"
type: workflow
---
只允许修改 `scripts/recent_mainlines.py`，以及确有必要时的 `scripts/archive_review.py`；不得修改 `index.html`、`data/` 或 workflow，不提交、不推送、不进行外部抓取。修改后需做语法检查，并用临时 fixture 验证去重、分母不超过 100%、缺失数据保持为 `null`，且不能把晚间生成的 premarket 冒充盘前数据。