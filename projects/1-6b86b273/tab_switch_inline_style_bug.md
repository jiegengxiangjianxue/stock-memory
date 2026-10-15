---
name: tab_switch_inline_style_bug
description: 排查网站「tab点击无反应」时读取：根因是内联style覆盖了class显隐切换，附修法与同类组件教训
---
# tab 点击无反应的根因与修法

## 症状
网站 stocks-ai-view 只有「每日总结」tab 有内容，点「全球股市/今日主线/多池选股」等 tab 无反应，画面不变。

## 根因（2026-10-15 定位并修复）
`index.html` 中 8 个 tab 卡片里，除第一张外全部带**内联** `style="display:none"`。
tab 切换逻辑（约 2111 行）只做 `classList.toggle('on', ...)`，而 CSS 规则 `.c-col .card.on{display:block}` 的优先级**低于**元素内联样式，所以：

- tab 按钮高亮正常（`.c-tab.active` 生效）
- 卡片 `on` 类也加上了
- 但内联 `display:none` 仍在，卡片永远不显示 → 用户看到「点了没反应」

浏览器实测确认：点击 tab 1 后 `card1.classList.contains('on')===true` 而 `card1.style.display==='none'`。

## 修法
删除 7 处 `<div class="card c-card" data-ci="N" style="display:none">` 里的内联 `style="display:none"`，统一交给 `.c-col .card{display:none}` + `.c-col .card.on{display:block}` 控制。
第一张卡片本身无内联样式（只有 `on` 类），所以一直正常。

## 教训
同类「class 切换控制显隐」的组件，不要混用内联 `style="display:none"` 做初始隐藏——内联样式优先级最高，class 切换会被它覆盖，且现象是「点了没反应」而非报错，极难自查。要么全用 class（含初始隐藏），要么在切换时显式清 `el.style.display=''`。

## 相关：另有两个 tab 是「渲染函数在、数据源不存在」(已修)
- 「今日主线」tab：`renderMainline()` 读 `g.mainline || g.hot_themes`，但 `SOURCES` 数组里没有 mainline.json / hot_themes.json，仓库里也从未生成过这两个文件，也没有对应采集脚本。
- 「热点主题」tab：`renderThemes()` 读 `g.themes || g.hot_themes`，同样缺数据源。

2026-10-15 已新增 `scripts/build_mainlines.py`，从 `news_impact.json` 聚合产出这两个文件（16 个主题 / 16 条主线），并接入 shard-collect.yml。同时修了 `renderThemes` 对 `{ts,themes:[...]}` 顶层结构与 `stocks:[{code,name}]` 的适配。

## 同源坑 2：pools 数组 key/src 错位 (2026-10-15)
`renderPools()` 的 pools 数组结构是 `[title, key, src, desc]`，但「推荐池」那行写的是
`['推荐池','recommend','picks',...]` —— 第二个元素 `recommend` 才是 key，第三个 `picks` 是 src。
于是 `key==='picks'` 永不匹配，推荐池 recs 恒为初始 `[]`，显示「暂无数据」。
修法：改成 `['推荐池','picks','ai_pick_v4',...]`。其余 8 个池的 key/src 原本就是对的。
**教训：数组元组按位置解构时，加新条目必须同时核对「第 2 位=分支 key」「第 3 位=g[] 取值键」。**

## 同源坑 3：调试代码插进 if/else 链 → 全站 JS 崩溃 (2026-10-15，犯了两次)
在 `if(...) recs=...;` 与 `else if(...)` 之间插入一行 `console.log(...)` 或 `document.title=...`，
会直接造成 `Uncaught SyntaxError: Unexpected token 'else'` —— 整个 `<script>` 不执行，
所有函数变 undefined，表现为「所有 tab 点击全无反应」，极易误判成 CSS/数据问题。

**教训：**
1. 在 if/else 链里加临时探针，必须写成 `if(cond){ probe(); }` 的完整语句，不能裸插一行。
2. 改完 index.html 立刻跑一次 `node --check` 抽检内联脚本（3 个 `<script>` 块），
   比在浏览器里反复试错快得多：
   `python3 -c "import re,subprocess,tempfile,os;[...]"`
3. 浏览器 console 的 error 面板第一条往往就是根因，先看它再猜。