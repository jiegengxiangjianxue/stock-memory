---
name: cloud_warehouse_architecture
description: stocks-db 云端数据仓的仓库路由、token 权限、采集脚本与已修坑位
type: workflow
---
名称：cloud_warehouse_architecture
摘要：stocks-db 云端数据仓架构、cloud-first 数据路由、token 权限、采集脚本与已修坑位
# 云端数据仓架构（2026-10-14 建立）

## 数据路由
项目采用 cloud-first 数据路由，本地 `data/` 目录已移除。仓库数据请求先访问 GitHub 云端仓库，GitHub raw 不可达时回退到 GitCode 镜像；实时行情由 `stock-detail.js` 直接访问腾讯或东方财富接口，`data_origin.js` 只负责仓库 JSON。

## 仓库路由（两个地方必须同步改）
`~/workspace/stocks-db/scripts/sync_repos.py` 与 `~/workspace/stocks-ai-view/db-client.js` 的 SHARD_REPO 映射。

| 仓 | 分片 |
|---|---|
| jiegengxiangjianxue1/db-market | master/market/sector/overseas + _index/_search |
| jiegengxiangjianxue1/db-stocks-a | 00 (1644) |
| jiegengxiangxue2/db-stocks-b | 30 (1453) |
| jiegengxiangxue2/db-stocks-c | 60 (1846) |
| jiegengxiangxue3/db-stocks-d | 68/15/51/50/52/56/58 (643) |
| jiegengxiangxue3/db-stocks-e | 20/81/83/89/90/92 (442) |
| dufuwuxin/stocks-db-market (dataset) | GitCode 镜像 |
| dufuwuxin/stocks-db-stocks (dataset) | GitCode 全量镜像 |

分片依据：单股票平均 229KB，���个 shard 一个仓，避免单仓超 1GB。

## token 权限（已实测）
- GitHub token 属于 `dufuwuxin`，不能在 `qwerzxcva`（他人 user 账号）下建仓，但对 3 个组织（jiegengxiangjianxue1/2/3）有 owner 权限 → 数据仓全部建在组织下。
- GitCode token 支持 `repository_type=dataset` 建数据集仓。

## 采集脚本
- `stocks-db/scripts/build_market_db.py` 大盘/板块/海外
- `stocks-db/scripts/build_stock_db.py` 个股（--all 全采，含网络）
- `stocks-db/scripts/build_overseas_live.py` 海外实时 + 新闻情绪 + A股影响
- `stocks-db/scripts/build_index.py` 索引层
- `stocks-db/scripts/sync_repos.py --push` 推送
- `stocks-db/scripts/cloud_bootstrap.py` Space 上物化云端输入给引擎

## 已修复的坑
1. bulk csv 双格式：`day_qfq_extra/recent` 是 `code|date,o,c,h,l,v` 逗号格式，其余是 7 字段管道。解析必须兼容两种。
2. 东财 kline `lmt=0` 返回 rc=102 data:null，必须用 lmt=100000。
3. `.stage/` 复用会因中断的 git 留下 dangling symlink 导致 rmtree 失败，每个目标用独立 stage 目录。
4. 新浪 A50（nhs_A50）返回空，改用东财 `100.XIN9`。
5. 新浪 roll feed `lid=2516`