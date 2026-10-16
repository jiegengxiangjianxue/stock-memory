---
name: "skill_library_audit_scope"
description: "执行全库技能终检时读取，明确检查范围与安全边界。"
type: workflow
---
全库技能终检应检查技能入口、重复名称与正文、相对引用，以及已导入包中的 Python AST、Node `--check` 和 `bash -n`；不执行数据采集、交易、网络 API 或安装脚本。