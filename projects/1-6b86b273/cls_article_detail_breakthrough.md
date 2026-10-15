---
name: cls_article_detail_breakthrough
description: 财联社 detail 页面免签拿文章全文（column=文章 content 达 8456 字符），VIP 内参需登录态未破；文章 id 需从首页/shareurl 反查
type: fact
---
# cls_article_detail_breakthrough — 财联社文章全文破甲（2026-10-16）

## 用户原话
「财联社的 vip 专享文章，你看看能不能逆向破甲一下」

## 结论：免签拿全文，VIP 墙不在这一层

`https://www.cls.cn/detail/{id}` 是 Next.js SSR 页，
`__NEXT_DATA__.props.pageProps.articleDetail` 免签返回完整文章（22 字段）。
**无需签名、无需 cookie、无需登录**。

### 关键字段
- `column.columnName`：`电报`（短，平均 200 字符）vs `文章`（长，实测 199~8456 字符）
- `isFree`：实测 **12/12 全 True**，没见过 False
- `level`：A/B/C
- `content`：完整正文（含 `【标题】` 前缀）
- 其他：`title`/`brief`/`ctime`/`author`/`readingNum`/`commentNum`/`images`/`assocVideoUrl`/`assocArticleUrl`

### 实测样本（2026-10-16）
```
[文章] id=2506108 len=8456  净利同比最高增长19.91倍！1100亿存储芯片股发布前三季度业绩预告|盘后公告
[文章] id=2506093 len=3449  美PPI超出预期一个百分点 纳指期货盘前跌超1% | 今夜看点
[文章] id=2505833 len=2589  【焦点复盘】大金融逆势护盘提振有限，创业板指跌近3%
[文章] id=2505784 len=3112  【每日收评】沪指跌1.59%险守3700点
[文章] id=2487344 len=3065  半导体大硅片论坛将至，行业规模稳步增长
```
一次采集 **63 篇文章 / 6 电报 / 0 失败**。

## VIP 专享为什么不在这层
- `v1/roll/get_roll_list` 返回 `data.is_vip`，实测恒为 0
- `category=vip/depth/neican/insider/premium` **全部静默降级**为空 category
  （返回同样 50 条，0 独有，errno=0）——服务端不报错，但不给数据
- 真正的 VIP 内参在 `/vip` `/depth` 栏目页，那些页面是 SPA，无 detail id 暴露
- **结论：电报+文章两层全免费；VIP 内参需要登录态，本次未破**

## 文章 id 来源（列表 API 全 404）
`v1/article/list`、`v1/article/get_article_list`、`api/article/list`、
`cache?name=articleList`、`nodeapi/telegraphList`、`m.cls.cn/*`（420）——**全不可用**。

只能用反查：
1. 首页 `https://www.cls.cn/` 的 `/detail/{id}` 链接（实测 12 个/页，约一半是文章）
2. 电报 v1 的 `shareurl`（`https://api3.cls.cn/share/article/{id}`）
3. id 邻域扫描（id 连续，约每 10 个有 1 篇文章）

## 限流
- HTTP **420** 是财联社限流码。m.cls.cn 全站 420
- 连打 detail 会触发，需 1.5~2.5s 间隔 + 重试退避
- 首页和 v1 列表不受影响

## 采集器
`~/workspace/stocks-ai-view/scripts/cls_article_collect.py`
```bash
python3 cls_article_collect.py --out /tmp/cls_art.json              # 首页反查
python3 cls_article_collect.py --ids 2506108,2506093 --out x.json   # 指定 id
python3 cls_article_collect.py --scan 20 --out x.json               # 邻域扫描
```

## 与 registry 的关系
`cls_telegraph_v1` 是电报列表（53 字段，但 title 恒空、content 短）；
`cls_article_detail` 是文章详情（content 长 10~40 倍）。**两者互补**，
文章才是 hot_impact / ai_review 的优质输入。