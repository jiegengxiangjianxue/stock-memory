---
name: tun_stack_retention_and_merge
description: 涉及 TUN 栈裁剪、合并或 tsbAI 优化时必须遵守的架构策略
---
TUN 栈仅保留 mixed、gVisor、system 和 tsbAI 四种实现；其他栈（如 Go、MIPS 等）全部合入 tsbAI，通过借鉴其代码、架构与设计来优化 tsbAI，不再独立保留。