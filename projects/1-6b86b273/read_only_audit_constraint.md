---
name: "read_only_audit_constraint"
description: "进行本项目审计时，保持只读并避免暴露敏感信息。 [待确认 2026-10-17，与 skills_audit_read_only 冲突]"
type: workflow
pending: true
---
审计任务约定：只检查和报告，不修改文件、不提交或推送代码；输出精确的文件与行号、已验证的数据流和缺口；不要回显密钥或其他敏感信息。