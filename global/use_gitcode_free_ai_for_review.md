---
name: use_gitcode_free_ai_for_review
description: 当需要决定用什么 AI 服务做每日复盘或修改复盘 prompt 时
type: identity
---
用户明确要求每日 AI 复盘必须调用 GitCode 免费 AI 额度（端点 https://gitcode.com/api/v5/chat/completions，Bearer token），而非 GitHub Models 等其他服务。主模型 deepseek-ai/DeepSeek-V4-Flash，备选 Qwen/Qwen3-30B-A3B-Instruct-2507。选择原因：分散配额压力、GitCode 每月送 AI 额度不用白不用、DeepSeek-V4-Flash 对中文金融文本效果好。复盘时必须结合本地算法与引擎产出的量化数据。