---
name: "commit_after_each_change_for_ci"
description: "改完代码、准备收尾时：用户要求每次修改都提交以触发 CI"
type: workflow
---
用户明确要求：每次改完都要提交，以便 CI 跑起来。这是本仓库的收尾动作，不是可选项。提交信息用 Conventional Commits（`type(scope): subject`，句末不加句号）。只提交本次相关改动；推送、改远端、提/合 PR 仍先问用户，不擅自 force push、reset --hard 或改写已有历史。