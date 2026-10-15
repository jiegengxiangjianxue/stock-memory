---
name: research_output_and_scope_rules
description: 调研类任务的输出路径、GitHub 抓取方式与写文件红线
type: workflow
---
研究产出写 `/root/workspace/stocks-data-live/data/research/<topic>_<yyyymmdd>.md` 与同名 `.json`；临时文件仅写 `/tmp/rea_parts/`。严禁修改 `scripts/` 或其它任何文件。GitHub 抓取：原始文件 `curl -s https://raw.githubusercontent.com/morluto/rea/main/<path>`；目录列表 `https://api.github.com/repos/morluto/rea/contents/<path>`（限频时 sleep 1）。每提炼 2-3 个文档必须立即追加写主文件，防进程中断丢成果。