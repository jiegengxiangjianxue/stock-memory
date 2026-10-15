---
name: sb_ai_kernel_architecture
description: 修改 sb-AI 内核、TUN 栈或相关接口时，读取项目指定的基础版本、命名与集成要求。
---
## 内核与 TUN 栈约定
- 当前使用 lxbox 的 singbox 内核；要求改以 lxbox 最新的 singbox-lx 为基础进行定制，不再沿用原上游。
- 定制内核命名为 `sb-lxf`，专供 `sb-AI` 使用，所有接口须对接到 `sb-AI`。
- 要求移植最新 singbox 官方 TUN 栈，重命名为 `tsbAI`，并参考 clash 新出的 mips 栈，借鉴其代码、架构与框架。
- 以上是明确提出的目标要求，对话未确认已完成实现或验证。