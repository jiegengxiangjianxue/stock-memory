---
name: sbai_command_client_cross_process
description: "sb-AI 的 SbCommandClient 如何跨进程连接 :core 内核，以及为何它不是进程内实例"
---
# sb-AI CommandClient 跨进程连接机制（已核实）

## 结论
`SbCommandClient.connect()` 调用 `Libbox.newCommandClient(handler, options)`（进程内签名），
**但实际连的是 `:core` 进程的 CommandServer**，不是 UI 进程自己的内核实例。

## 核实依据（来自 app/libs/libbox.aar 的 native 字符串）
- `failed to connect to %s` —— newCommandClient 走 socket 连接，不是进程内直调
- `sing-box/route.sock`、`sing-box/common/process.sock` —— socket 路径带 `sing-box/` 前缀，
  即 `Libbox.setup` 的 basePath 下的相对路径
- 两进程 basePath 相同（都是 `filesDir/sing-box`），因此 UI 进程 setup 后能连到 :core 的 socket

## 关键代码
- `app/src/main/java/com/sbai/service/SbCommandClient.kt:115` — `Libbox.newCommandClient(this, options)`
- `app/src/main/java/com/sbai/ui/home/HomeScreen.kt:291-300` — 注释说明必须先调
  `LibboxRuntime.setup()`（幂等）才能连上，且 connect() 阻塞必须在 IO 线程
- `app/src/main/java/com/sbai/service/LibboxRuntime.kt:35` — `Libbox.setup(SetupOptions{basePath...})`

## 踩坑记录
曾怀疑「UI 进程用 newCommandClient 会自己跑一个内核实例导致内存翻倍」，
经 native 字符串核实**不成立**。若再遇到此类怀疑，先查
`failed to connect to` 与 `sing-box/*.sock` 路径前缀再下结论，不要按 API 名字想当然。

## 遗留疑点（未验证）
- `Libbox.newRemoteCommandClient` + `RemoteConnectionOptions` 才是显式跨进程 API，
  当前用的是 `newCommandClient` + 共享 basePath 的隐式方式。两种方式在当前内核等价，
  但未做真机抓 socket 验证。