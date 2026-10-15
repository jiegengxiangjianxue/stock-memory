---
name: json_output_additive_only
description: 修改任何输出 JSON 的脚本时，必须遵守的字段变更规则
---
stocks-ai-view 项目中，所有脚本的 JSON 输出结构遵循「只增不改」原则：已有字段名、类型、语义不得修改或删除，只允许新增字段。适用于 engine_v5.py、reflect_engine.py、ban_pick.py、ban_prob.py 等所有输出 JSON 的脚本。