---
name: space_cluster_cross_keepalive
description: Space 30分钟休眠问题的解法：多组织交叉保活，3组织×1 Space按职责分摊核时
---
# Space 休眠与多组织交叉保活（2026-10-14 用户指示）

**现状**：GitCode Space 每 30 分钟无 HTTP 访问就休眠。单 Space 保活不可靠
（唤醒只损失延迟不损失数据，但用户要求 24h 不间断）。

**用户指示**：多开几个组织，每个组织里开 Space，交叉使用。

## 既定方案（见 docs/SPACE_CLUSTER.md）

3 个组织 × 1 个 Space，按职责分：

| 组织 | Space | 职责 | 频率 | 月核时 |
|---|---|---|---|---|
| A | stocks-crawl | 实时采集（行情/涨跌停/板块/新闻/资金流） | 每 5 分钟 | ~180 |
| B | stocks-engine | 算法引擎（多维/决策/回测/估值） | 每 15 分钟 | ~240 |
| C | stocks-ai | 复盘 + AI + 看板 | 每小时 | ~120 |

核时账：2 核常驻单组织月耗 1440 > 1000 配额；3 组织分摊每组织 480，留 52% 余量。

## 保活机制

- 每个 Space 内置 `/health` 端点（app.py 已挂 FastAPI 路由）
- GitHub Actions `space-pull.yml` 每 20 分钟 curl 三个 Space 的 /health 交叉保活
- Space 内 `health_loop` 每 4 分钟自写 heartbeat.txt

## 待办

- [ ] 组织 B、C 需用户在网页创建（或提供授权）
- [ ] 创建后把 Space 运行 URL 填进 full-pipeline.yml 的 SPACE_URL
- [ ] space-pull.yml 的保活 curl 目标补全三个组织