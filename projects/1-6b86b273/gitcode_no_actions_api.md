---
name: gitcode_no_actions_api
description: GitCode 平台无 Actions API，CI 必须跑在 GitHub 再镜像
type: fact
---
GitCode 平台没有 Actions API（访问返回 404），因此本项目所有 CI/CD 必须跑在 GitHub Actions，再将产物通过 git 推送到 GitCode 仓库进行镜像。涉及仓库：GitHub `qwerzxcva/stocks-ai-view` ↔ GitCode `dufuwuxin/stock-system`。