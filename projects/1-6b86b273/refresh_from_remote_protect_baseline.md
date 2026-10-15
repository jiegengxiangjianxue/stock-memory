---
name: refresh_from_remote_protect_baseline
description: 使用或排查 refresh_from_remote --protect 覆盖/失效问题时必读
type: fact
---
`refresh_from_remote(paths, dirs, all_changes, branch, protect, base_ref)` 中，`--protect` 传入的文件只有与 `base_ref`（默认 `HEAD`）版本逐字节不同时才会被备份并恢复；若与基线一致，会被判定为「本轮未改动」而跳过保护。

**已确认坑（2026-10-14）**：`rebuild-pools` 因此把 `ai-update` 13:19 生成的 `latest.json` 覆盖回 10:49 的旧版。

**正确做法**：要保护本轮生成的文件，需保证它在 run 内相对 `base_ref` 有实际内容变更，或显式传入更早的 `base_ref`（如 `HEAD~1`）作为基线。