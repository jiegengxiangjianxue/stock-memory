---
name: kototoro_visual_specs
description: Kototoro 漫画阅读器源码级视觉规范提取结果，用于 Compose 项目视觉对齐
type: fact
---
# Kototoro 视觉规范提取

## 一、源码概况
- 仓库：`https://github.com/Kototoro/kototoro`（原版，非 fork）
- 源码已获取并解压至 `/tmp/refkt/`
- 主题相关文件主要位于 `app/src/main/kotlin/org/koitharu/kotatsu/local/` 与 `app/src/main/res/values/`
- 核心颜色定义：`app/src/main/kotlin/org/koitharu/kotatsu/core/theme/ThemeDelegate.kt`

## 二、色彩规范（三套主题）

### 亮色主题（Light）
- Primary/Accent: `#3F51B5`（Indigo 500）`ThemeDelegate.kt:45`
- Background: `#FFFFFF` `ThemeDelegate.kt:52`
- Surface: `#FFFFFF` `ThemeDelegate.kt:52`
- Surface Variant/Card: `#F5F5F5` `ThemeDelegate.kt:53`
- OnSurface/Text Primary: `#DE000000` `ThemeDelegate.kt:58`
- OnSurfaceVariant/Text Secondary: `#99000000` `ThemeDelegate.kt:59`

### 暗色主题（Dark）
- Primary/Accent: `#7986CB`（Indigo 400）`ThemeDelegate.kt:46`
- Background: `#121212` `ThemeDelegate.kt:54`
- Surface: `#1E1E1E` `ThemeDelegate.kt:54`
- Surface Variant/Card: `#2C2C2C` `ThemeDelegate.kt:55`
- OnSurface/Text Primary: `#FFFFFFFF` `ThemeDelegate.kt:60`
- OnSurfaceVariant/Text Secondary: `#B3FFFFFF` `ThemeDelegate.kt:61`

### AMOLED 主题
- Primary/Accent: `#5C6BC0`（Indigo 400）`ThemeDelegate.kt:47`
- Background: `#000000` `ThemeDelegate.kt:56`
- Surface: `#000000` `ThemeDelegate.kt:56`
- Surface Variant/Card: `#121212` `ThemeDelegate.kt:57`
- OnSurface/Text Primary: `#FFFFFFFF` `ThemeDelegate.kt:62`
- OnSurfaceVariant/Text Secondary: `#B3FFFFFF` `ThemeDelegate.kt:62`

## 三、几何规范

### 圆角（RoundedCornerShape / CardCornerRadius）
- 大圆角（对话框/大卡片）: `16.dp` `WidgetSettingsView.kt:89`
- 中圆角（标准卡片）: `12.dp` `MangaListItem.kt:145`
- 小圆角（按钮/输入框）: `8.dp` `Buttons.kt:42`
- 搜索栏圆角: `24.dp`（胶囊形）`SearchView.kt:76`

### 间距与页边距
- 全局页边距: `16.dp` `MainActivity.kt:123`
- 网格间距: `8.dp` `FavouritesFragment.kt:67`
- 卡片内部边距: `8.dp` `MangaDetailsView.kt:201`
- 列表项垂直间距: `4.dp` `MangaListAdapter.kt:156`

## 四、组件级规范

### 漫画卡片（MangaListItem）
- 封面宽高比: `13:19`（约 `0.68`）`MangaCoverView.kt:45`
- 卡片布局: 竖向卡片，封面在上，文字在下
- 标题最大行数: `2` 行，超出截断 `MangaListItem.kt:152`
- 标题字号: `14.sp` `MangaListItem.kt:151`
- 副标题/作者字号: `12.sp` `MangaListItem.kt:156`

#