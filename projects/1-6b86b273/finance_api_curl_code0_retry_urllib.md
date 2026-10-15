---
name: finance_api_curl_code0_retry_urllib
description: 复核财经接口时，curl 返回 0 先不要判死，改用 Python urllib 复测。
type: fact
---
本记录中 `tencent_board_rank` 等端点先用 curl 探测返回 `http_code=0`、`0B`，但 DNS 可解析；改用 Python `urllib` 并带浏览器 UA 后，`tencent_board_rank` 实测 `200 95457B 0.17s`。因此对 `proxy.finance.qq.com`、`finance.sina.com.cn`、`stock.finance.sina.com.cn`、`web.ifzq.gtimg.cn` 等域名，curl 0 时应以 urllib 或其他客户端复测后再下结论。