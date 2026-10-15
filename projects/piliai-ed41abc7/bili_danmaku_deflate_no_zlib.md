---
name: bili_danmaku_deflate_no_zlib
description: B站弹幕接口 deflate 响应缺 zlib 头，OkHttp 无法自动解压，需应用层手动处理
type: fact
---
调用 `x/v1/dm/list.so` 时，响应声明 `Content-Encoding: deflate`，但数据流缺少 zlib 头（前两字节不是 `0x78 0x9c` 之类），OkHttp 的自动解压处理不了会原样返回，必须在应用层手动解压。已解压的 XML 以 `<?xml` 开头。