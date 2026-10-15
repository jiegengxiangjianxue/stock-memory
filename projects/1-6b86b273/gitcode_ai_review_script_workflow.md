---
name: gitcode_ai_review_script_workflow
description: 当需要运行、修改或调试 GitCode AI 复盘脚本时
type: workflow
---
scripts/gitcode_ai_review.py 负责调用 GitCode AI 生成每日复盘。输入为 data/ 下的本地 JSON（market_now、multidim、decision、reflect、self_reflect、news_digest），不发额外请求；输出 data/ai_review.md 和 data/ai_review.json。运行方式：GITCODE_TOKEN=<token> python3 scripts/gitcode_ai_review.py；加 --dry-run 可只打印 prompt 不调用。模型主选 deepseek-ai/DeepSeek-V4-Flash，备选 Qwen/Qwen3-30B-A3B-Instruct-2507。