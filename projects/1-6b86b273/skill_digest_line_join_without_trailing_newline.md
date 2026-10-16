---
name: "skill_digest_line_join_without_trailing_newline"
description: "复核本地 skill 文件快照明细时，记录摘要按去尾换行的行拼接计算，而非文件原字节"
type: fact
---
在对比 `.agents/skills/` 上游快照与本地安装文件时，记录的 SHA256 摘要对应 `digest_without_trailing_newline`（即去除尾随换行后按行拼接的正文），不是含尾随换行的文件字节。复核时若直接计算文件原始 SHA256 会得到 `matches_recorded_digest: false`，需切换到“行拼接、无尾换行”口径才能对齐。