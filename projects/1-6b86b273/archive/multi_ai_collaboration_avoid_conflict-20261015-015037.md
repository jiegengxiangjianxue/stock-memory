---
name: multi_ai_collaboration_avoid_conflict
description: 项目存在另一个 AI 助手并行修改代码，改动时需避免文件冲突并保证最终可用
type: workflow
---
项目中有另一个 AI 在同时跑。任何改动前需确认目标文件是否正被另一方修改；若必须涉及同一文件，需在内容层面避免冲突（如追加而非覆盖、保持语法完整），且改完后文件必须能正常使用。推送前应检查远端是否有新提交，必要时先 pull 再合并。