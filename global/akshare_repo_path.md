---
name: akshare_repo_path
description: akshare 源码位置与接口挖掘用途
type: fact
---
akshare Python 包源码在 `/usr/local/lib/python3.14/dist-packages/akshare/`，可用 `grep -rn "sina.com.cn\|gtimg\|hexun"` 等命令提取财经接口 URL 作为调研起点；但 akshare 中部分接口可能已失效，必须逐一实测验证。