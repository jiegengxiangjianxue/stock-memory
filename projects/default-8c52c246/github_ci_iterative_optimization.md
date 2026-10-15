---
name: github_ci_iterative_optimization
description: 代码优化需同步推送 GitHub，并用 CI 暴露本地未发现的问题
type: workflow
---
用户要求：重要改动不要只留在本地，需同步推送到 GitHub；利用 GitHub CI 运行构建、测试、静态检查等验证，主动发现本地发现不了的问题；发现失败后继续修复和优化，直到 CI 结果可接受。