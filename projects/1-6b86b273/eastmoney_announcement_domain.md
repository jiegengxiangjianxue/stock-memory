---
name: eastmoney_announcement_domain
description: 修复东财公告抓取返回 HTML/SPA 壳问题时读取。
type: fact
---
东财公告抓取必须使用 `https://np-anotice-stock.eastmoney.com/api/security/ann`，参数示例：`page_index=1&page_size=5&ann_type=A&client_source=web&f_node=0&s_code={code}`。使用 `np-an.eastmoney.com` 会返回 SPA/HTML 壳，拿不到公告 JSON。