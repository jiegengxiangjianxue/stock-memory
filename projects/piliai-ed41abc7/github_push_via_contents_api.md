---
name: github_push_via_contents_api
description: 当需要向 piliai 仓库推送代码且直连 github.com 失败时
type: workflow
---
github.com:443 被墙，git push/SSH 全部失败；api.github.com 可正常访问。推送到该仓库时必须使用项目内的 .push_via_api.py 脚本，通过 GitHub Contents API 上传文件；批量改动应合并为单次提交（如 Git Data API），避免逐文件触发大量 CI。