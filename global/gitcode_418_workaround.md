---
name: gitcode_418_workaround
description: gitcode.com 直连被反爬拦截时的绕行方案
type: fact
---
gitcode.com 直连会返回 HTTP 418（反爬拦截）。访问 GitCode 上的 GitHub Trending 镜像仓库时，应改用对应的 GitHub 原仓库名（如 `apache/doris`），或使用 `raw.githubusercontent.com` 直接获取文件内容，避免直连 gitcode.com。