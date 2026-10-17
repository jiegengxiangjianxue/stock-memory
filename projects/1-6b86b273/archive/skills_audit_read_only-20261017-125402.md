---
name: "skills_audit_read_only"
description: "进行技能盘点时应遵循的只读审计流程 [待确认 2026-10-17，与 cloud_only_deployment_pending_73296d4552e6 冲突]"
type: workflow
pending: true
---
盘点 `/root/.aharou/skills/1` 下技能时，仅读取目录、SKILL.md、脚本、references、requirements、package.json 和 `_meta.json`；不得写文件、运行脚本或联网。报告需按行情数据、组合风险、回测、交易/社区、金融 API、QA/平台分组，列出技能目录与声明名、完整文件清单、脚本入口和依赖，并基于真实路径和行号指出完全重复、功能重叠但需保留、可合并内容，以及失效引用或缺失依赖；带脚本的技能需要重点保护。