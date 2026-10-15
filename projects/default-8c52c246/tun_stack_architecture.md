---
name: tun_stack_architecture
description: TUN 栈保留与整合策略：仅保留四种实现，其余合入 tsbAI 用于借鉴
type: workflow
---
项目中 TUN 栈仅保留 mixed、gVisor、system 和 tsbAI 四种实现。其他如 Go、MIPS 等实现应合入 tsbAI 中，目的是借鉴其代码、架构、设计来优化 tsbAI，不作为独立 TUN 栈保留。