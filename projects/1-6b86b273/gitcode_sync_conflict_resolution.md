---
name: gitcode_sync_conflict_resolution
description: GitCode 推送遇历史分叉时的绕行方案
type: fact
---
GitCode 仓库历史可能与 GitHub 侧分叉，推送前必须先执行 fetch，然后 `git merge -X ours` 保留 GitHub 侧变更，最后使用 `git push --force-with-lease` 完成镜像。