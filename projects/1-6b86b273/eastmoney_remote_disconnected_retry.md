---
name: eastmoney_remote_disconnected_retry
description: 调用东财 push2 接口遇 RemoteDisconnected 时的重试参数
type: fact
---
东财 `push2.eastmoney.com/api/qt/clist/get` 对连续请求会直接 `RemoteDisconnected`。仅靠短退避（如 0.3s）重试无效；需把重试次数提到 3 次、退避间隔设为 3 秒（实测 0.3s 间隔连丢 2 次，3s 后第 3 次即通）。项目内 `_http` 已按此调整。