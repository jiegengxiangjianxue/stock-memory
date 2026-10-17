---
name: "page_json_stale_marker"
description: "新增或维护页面消费的 JSON 时，先确认其包含统一的新鲜度标记。 [待确认 2026-10-17，与 verify_uncommitted_data_provenance 冲突]"
type: workflow
pending: true
---
项目约定：每个被页面消费的 JSON 都应携带 `stale` 标记，并包含 `age_hours` 与对应阈值，用于前端和审计判断数据是否过时。