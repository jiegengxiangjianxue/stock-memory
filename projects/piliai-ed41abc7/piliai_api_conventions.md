---
name: piliai_api_conventions
description: piliai B站接口的风控防护、写操作校验与错误码翻译约定，改 API 层前必读
---
# piliai API 层风控与错误处理约定

改 B 站接口相关代码前必读。

## 一、HTTP 层防护（BiliHttpClient.kt）
- `HttpClient.getSafe<T>(path) { ... }` 扩展：与 `client.get` 签名兼容
  （path + 尾随 lambda），内部先读文本、检测 HTML、再反序列化。
  **新增 GET 接口一律用 `getSafe`，不要用裸 `client.get(...).body()`**——
  风控返回 HTML 时会抛「Unexpected JSON token ... had '<'」，
  且会绕过上层「code != 0 才降级」的降级链。
- `getSafe` 的 Json 实例从 `BiliHttpClient.json` 取，**不要加成参数**——
  否则尾承 lambda 会被绑到错误参数上（Kotlin 尾随 lambda 只对最后一个参数生效）。
- 非 JSON 响应（弹幕 protobuf/XML、WebSocket）不走 ContentNegotiation，不受影响。

## 二、写操作校验（BiliApiClient.kt）
- `requireWriteOk(resp)`：写操作统一用。code=0 返回 true，否则抛出自带
  可读 message 的异常（经 `BiliErrorTranslator.translate` 翻译）。
- **不要写 `JSONObject(resp).optInt("code") == 0`**——失败只返回 false，
  上层只能显示「发送失败」。
- CSRF：`postAuthForm` 自动加 `csrf` 字段。**直播弹幕接口的字段名是
  `csrf_token`**，必须自己从 `AccountSession.snapshot()["bili_jct"]` 取
  （审核288 修过传空串导致必失败 -111 的 bug）。

## 三、错误码翻译（BiliErrorTranslator）
已覆盖：-352 风控 / -403 需登录 / -101 未登录 / -404 失效 / -799 频繁 /
62012 评论区关闭 / -509 频繁 / -104 硬币不足 / 34002 不能给自己投币 /
-111 csrf 失效 / v_voucher 验证。
新增错误码往这个 when 里加。

## 四、UI 层错误展示
- `VideoPlayerViewModel.setError()` 已统一过 `BiliErrorTranslator.translate`
- `CommentViewModel` 的 onFailure 也要翻译后展示
- 异步多步操作（如一键三连）**不要预设成功 toast**——子步骤可能失败，
  会同时显示「成功」和「失败」自相矛盾（审核289 修过）

## 五、验证命令
```
cd ~/workspace/piliai/android
./gradlew :app:compileDebugKotlin --no-daemon --offline -q
./gradlew :app:lintDebug --no-daemon --offline
cd .. && python3 tests/check_lint_report.py build/app/reports/lint-results-debug.xml
```