---
name: "persist_bilibili_buvid"
description: "处理 B 站请求在应用重启后出现 -352/-412 等风控错误时应读取此约定。"
type: fact
---
B 站的 buvid3/buvid4 需要持久化到 DataStore，而不能只保存在内存中；否则进程重启后会丢失匿名风控指纹，B 站会将应用视为新设备，搜索、点赞和推荐流更容易出现 -352/-412。