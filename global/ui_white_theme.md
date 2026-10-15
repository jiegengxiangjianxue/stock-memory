---
name: ui_white_theme
description: 网页 UI 需保持白色/浅色主题，不要改回深色
type: preference
---
用户明确要求网页背景使用白色，并参考 QuantPlay 的设计风格。已在 CSS 变量中落实：`--bg:#f6f8fa`、`--panel:#ffffff`、`--panel2:#f0f3f7`、`--tx:#1f2328`、`--border:#d8dee6` 等（2026-10-15 更新）。后续涉及该网页的样式改动应保持白底/浅色主题，不要回退为深色背景。