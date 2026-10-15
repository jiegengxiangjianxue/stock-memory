---
name: piliai_home_visual_specs
description: piliai 首页视觉重构的 Kototoro 源码级规格与改动方向，动首页 UI 前必读
---
# piliai 首页视觉重构规格（Kototoro 源码实测，/tmp/refkt/Kototoro-devel）

> 用户多次反馈 piliai 首页「廉价、差距太大」。前几轮只调间距/圆角参数无效。
> 本轮改为按 Kototoro 源码级规格重构视觉语言。以下数值全部实测自源码。

## 一、网格规格（KototoroContentListScreen.kt）
- `GridSpacing = 8.dp`（:117）
- `GridHorizontalPadding = AppLayoutTokens.compactItemHorizontalPadding = 12.dp`（:116）
- `AppLayoutTokens`: screenHorizontalPadding=12.dp / sectionHorizontalPadding=12.dp / sectionVerticalSpacing=16.dp / compactItemHorizontalPadding=12.dp（CompactTopBarChrome.kt:7-12）
- 列数不是固定 2：`resolveGridColumnCount(availableWidth, targetItemWidth, spacing)`（:742）
  - `minCellWidth = targetItemWidth * 0.94f`（GridColumnMinFitRatio，:113）
  - `floor((availableWidth + spacing) / (minCellWidth + spacing))`
- 列宽会拉伸填满：`resolveGridPosterCardStyle`（:720）算出 cellWidth 后按比例缩放 posterHeight
  → **这是关键**：Kototoro 卡片宽度=可用宽度/列数，不写死 dp

## 二、卡片规格（SharedContentCardStyle.kt）
- `ContentCoverCornerRadius = 12.dp` / `CompactContentCoverCornerRadius = 8.dp`
- `moviePosterCardStyle`（横版内容，最接近 B 站视频）：
  - `itemWidth = (176 * scale).dp.coerceIn(160.dp, 264.dp)`（scale∈[0.5,1.5]）
  - `posterHeight = itemWidth * (9/16)`
  - `cornerRadius = 12.dp`
- `compactPosterCardStyle`（竖版漫画）：
  - `itemWidth = (96*scale).coerceIn(48,134).dp`
  - `posterHeight = (136*scale).coerceIn(68,190).dp`

## 三、封面 + 标题（SharedPosterCover.kt）—— piliai 最缺的一环
- 封面容器：`shadow(2.dp, shape, clip=false)` + `clip(shape)` + `background(surfaceVariant)` + `border(0.5.dp, rimBorderBrush, shape)`
  → **0.5dp 描边 + 2dp 阴影**是「廉价感」变「质感」的关键
- **compact overlay 标题**：标题压在封面底部，不是放在封面外下方
  - overlay 高度 = `posterHeight * 0.42f`，coerceIn(54.dp, 72.dp)（:129）
  - 渐变色标（:52-60）：0→透明，0.2→黑5%，0.4→黑16%，0.6→黑34%，0.8→黑54%，1.0→黑72%
  - 标题字：`labelMedium.copy(fontSize=resolveCompactGridTitleFontSize, lineHeight=fontSize+3.5.sp, SemiBold)`，白色，maxLines=2
  - **带文字阴影**：Shadow(color=黑75%, offset=(0,1.5), blurRadius=5f)
  - padding：horizontal 8.dp / vertical 6.dp
- 标题字号随网格缩放：`resolveGridTitleFontSize(scale) = 12 + 4*normalized` sp（:133）；compact 版再 -1.5sp

## 四、Section header（HomeUi.kt:204）
- 标题 `titleSmall` + `FontWeight.SemiBold` + onSurface，maxLines=1
- 右侧计数 `labelMedium` + onSurfaceVariant
- 箭头 Icon 16.dp（large 20.dp），tint onSurfaceVariant
- 整条是 TextButton（contentPadding=0），SpaceBetween 布局

## 五、不可照搬（漫画特有）
- 书脊 ContentCardBookSpine（3.5dp 竖向渐变，仅 posterHeight>itemWidth 时）
- 13:19 竖封面比例（B 站视频是 16:9，用 moviePosterCardStyle 的 9/16）

## 六、piliai 现状（改前）
- 首页 `LazyVerticalGrid(GridCells.Fixed(2))`，卡距 10dp、页边 12dp（HomeScreen.kt）
- VideoCardItem：Card(surfaceContainerLow, 14dp 圆角, 0 elevation) + 封面 16:9/12dp 圆角 + 下方文字区（titleSmall/SemiBold 标题 + 头像行）
- 已有：封面底部 36dp 渐变 + 播放量/弹幕/时长角标

## 七、已落地（审核280~287，全部编译+lint 通过、CI 绿）
1. **VideoGridCard 组件**（PiliComponents.kt，公开）：网格视频卡，封面 rim 描边 +
   2dp 阴影、标题封面内 overlay（6 段色标 + 文字阴影）、角标左上/右上、
   UP 主行在封面外。**首页与分区页共用这一份**（原来 ZoneCard 是旧版副本）
2. **VideoListRow 组件**（PiliComponents.kt，公开）：横向列表行，移植
   HomeListRailRow；带 onLongClick（combinedClickable，仅传入时启用）。
   排行榜/热门/搜索/动态/空间/收藏历史全用它
3. **SectionHeader 组件**：移植 HomeSectionHeader
4. **CategoryPillStrip**：单条横滑胶囊，取代「两个 SegmentedPillRow 塞 LazyRow」
5. **rememberCoverRimBrush()**（Theme.kt）：封面 rim 渐变刷，全站封面共用
6. **PiliCoverScrimStops**（Theme.kt）：6 段渐变色标
7. **首页网格列数自适应**：resolveGridColumnCount（HomeScreen.kt），
   手机 2 列 / 平板 3~4 列；骨架屏与网格共用列数
8. **Hero 遮罩**改 HomeHeroArtworkScrim 规格（20%→56% 双段）
9. **封面规格补齐的页面**：番剧详情大封面、直播网格卡、离线缓存封面、
   PGC 竖版海报、动态视频引用块与转发块封面

## 七之补、公共组件清单（改视觉前先看有没有可复用的）
- `ui.components.VideoGridCard` — 网格卡（首页/分区）
- `ui.components.VideoListRow` — 横排列表行（带 onLongClick）
- `ui.components.SectionHeader` — 区块标题
- `ui.components.CategoryPillStrip` — 横滑分类胶囊
- `ui.components.SearchPill` / `SegmentedPillRow` — 搜索胶囊 / 分段控件（设置页用）
- `ui.theme.rememberCoverRimBrush()` — 封面描边刷
- `ui.theme.PiliCoverScrimStops` — 封面渐变色标
- `ui.theme.PiliCardShape` / `PiliTokens` — 圆角与间距 token

## 八、piliai 硬编码色值清查结论
全 App 带色硬编码仅剩 3 处，均有正当语义，**不要改**：
- `LiveRoomScreen.kt` SC 卡片背景（服务端下发，解析失败兜底红）
- `LiveAreaScreen.kt` 分区标签（B 站品牌粉 0xCCFB7299）
- `LiveAreaScreen.kt` 观看人数（黑底白字叠封面）
排名色已全部改为 primary 透明度阶梯（原来 0xFFFB7299/0xFFFF8C42/0xFFF5C518 三个
硬编码，换主题后不跟随）。

## 九、验证方式（每次 UI 改动必跑）
```
cd ~/workspace/piliai/android
./gradlew :app:compileDebugKotlin --no-daemon --offline -q
./gradlew :app:lintDebug --no-daemon --offline
cd .. && python3 tests/check_lint_report.py build/app/reports/lint-results-debug.xml
```
lint 产物在**仓库根** `build/`，不在 `android/app/build/`（CI 里 `cd android` 后
`find ..` 找的就是根 build）。