---
name: stocks_ai_view_no_modify_constraint
description: 在 stocks-ai-view 中新增功能时确认是否可改现有代码
---
项目约定：不修改现有已运行的代码逻辑，只新增文件、不破坏现有接口。新增脚本放 `scripts/`，数据库变更通过 `CREATE INDEX IF NOT EXISTS` 等幂等操作追加，不 drop 现有表/列。git 提交用 conventional commit，如 `feat: ...`。