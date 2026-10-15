---
name: webfetch_github_pypi_blocked
description: webfetch 无法访问 GitHub/PyPI 等站点时，改用 websearch 获取仓库元数据
type: fact
---
本环境 webfetch 访问 github.com、api.github.com、raw.githubusercontent.com、pypi.org 及部分官方文档站点（如 lumibot.lumiwealth.com）会返回「拒绝访问本机或内网地址」。需要仓库 star/license/默认分支/语言等元数据时，直接用 websearch 搜索仓库 URL 或关键词，从搜索结果摘要提取即可；无法访问源码时如实标注「无法访问」，不得编造。