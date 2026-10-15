---
name: sbai_crash_and_jank_root_causes
description: 排查 sb-AI 闪退/卡顿/配置丢失前必读：已定位并修复的 3 个崩溃根因、4 个卡顿根因、1 个数据丢失高危点，含文件与机制。避免重复怀疑已排除的方向。
type: fact
---
# sb-AI 闪退与卡顿的已修根因（2026-10-14）

按「先定位根因再修」的原则，以下每条都已定位到具体文件与机制，不要凭记忆重复怀疑。

## 闪退（3 个已修根因）

1. **ForegroundService 超时崩溃** — `SbAiVpnService.onStartCommand`
   原来只有 `ACTION_START -> startVpn()` 路径会调 `startForegroundWithNotification()`，
   而 `startVpn()` 有多个提前 return（`runtime != null`、`_status` 是 Running/Starting/Stopping）。
   用户连点两次启动时，第二次 `startForegroundService` 进来后在 startForeground 之前就 return，
   服务永不提前台 → `ForegroundServiceDidNotStartInTimeException` 杀进程。
   修法：`onStartCommand` 一进来就无条件提前台，再分发动作（同 id 重复调用只是更新通知）。

2. **backdrop RuntimeShader API 33 on minSdk 26** — vendored kyant backdrop 库的
   `RuntimeShader` 是 API 33+ 类。调用点虽有 `isRuntimeShaderSupported()` 运行时守卫，
   但 lint 的 NewApi 跨模块不认自定义守卫。Android 12 及以下走到任何 blur 路径都会
   `NoClassDefFoundError` 崩。修法：给 4 个方法补 `@RequiresApi(TIRAMISU)`
   （RuntimeShaderCache.obtainRuntimeShader / Paint.setRuntimeShader /
   HighlightStyle.createShader / BackdropEffectScopeImpl.obtainRuntimeShader）。
   另：底栏已改用半透明 Surface，不再走 blur（见下）。

3. **MainActivity 覆盖层与导航状态并存** — NavHost 外层 Box 直接叠加
   `SettingsLoadBalanceScreen` 全屏页。已删除该覆盖层，负载均衡改由设置页内二级页承载。

## 卡顿（4 个已修根因）

1. **layerBackdrop 全页录制** — `MainActivity` 对整个 NavHost 挂 `Modifier.layerBackdrop`，
   每帧都要录制整页 GraphicsLayer 供底栏 blur 采样。首页节点列表滚动时等于每帧多一次
   全屏离屏渲染 + blur，是首页卡顿的最大来源。已移除 layerBackdrop 与
   `SbGlassBottomBar` 的 `drawBackdrop/blur`，改半透明 Surface + 阴影。

2. **测速回填同步 commit() 阻塞主线程** — `NodeBatchTester.applyResults` 与
   HomeScreen 单节点测速用 `store.updateCommitted`，它同步 `commit()` 写整份 AppState JSON
   （含所有节点原始 outbound JSON，可达数 MB）到磁盘，主线程阻塞数秒。
   已改异步 `store.update`（延迟值只服务 UI 展示，`:core` 下次启动会 reload 读盘）。

3. **remember key 用整个 List** — HomeScreen 的 `filteredNodes` 与 `nodesBySubscription`
   的 remember key 是 `state.proxyNodes`，节点任何字段变化（测速回填/自动禁用）都触发
   全量 filter+sort / groupBy。已改为字段投影 `FilterKey(name, outboundJson,
   urlTestDelay, subscriptionId, enabled)` 与 `(id, subscriptionId)` 结构 key。

4. **底栏参数每次重组都新建** — `items.map { SbNavItem(...) }` 每次重组新建 list + 4 个实例，
   `SbGlassBottomBar` 参数永远不相等，跟着 VpnToggleButton 的 1 秒轮询一起重组。
   已提为 `remember(items)` 并给 `SbNavItem` 标 `@Immutable`。

## 高危数据丢失（已修）

`RuleStore` 的 Json 只有 `ignoreUnknownKeys`（只忽略未知 key，不处理未知枚举值）。
任何「删枚举值 / 改枚举值 / 配置来自更新版本」都会让 `decodeFromString` 抛
`SerializationException` → `load()` 回退 `AppState()` → 用户全部配置静默丢失。
已加 `coerceInputValues = true`（备份导入的 Json 同步加）。
因此删除已序列化枚举值是**禁止**的，只能标 `@Deprecated`（参考 `ResourceType` 的做法）。

## 未确证 / 待验证

- 用户侧闪退是否已全部消失，需要装包复现 + logcat 确认。以上 3 个根因都是静态分析 +
  代码路径推导得出，没有真实崩溃堆栈佐证。