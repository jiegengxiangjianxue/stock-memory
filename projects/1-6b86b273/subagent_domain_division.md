---
name: subagent_domain_division
description: 子代理域认领+探测节流机制，派发任何子代理前必读，防止重复抓取浪费额度
type: workflow
---
# 子代理域认领+探测节流机制（2026-10-13 建立，用户强制要求）

## 用户原话
「不要让每个账号抓取同样的数据，要分开抓取，分开搜索，减少重复和浪费原有的 API 额度」

## 背景（踩坑）
三个子代理并行都去搜「股票数据 API」、都去测东财/新浪/雪球，合计烧掉 2900 万 token，结论大量重复。还有一个子代理烧了 1147 万 token 后文件都没落盘（被写租约拦 + 不早落盘）。

## 两个机制（必须遵守）

### 1. 域认领表 `/root/workspace/stocks-ai-view/scripts/domain_lease.py`
14 个域：realtime_quote / kline / capital_flow / financial / news / social / fund_etf / macro / events / exchange_official / index / sector / algo / paid_api

```bash
python3 /root/workspace/stocks-ai-view/scripts/domain_lease.py --available              # 派发前查
python3 /root/workspace/stocks-ai-view/scripts/domain_lease.py --claim <agent> <domain> # 必须认领
python3 /root/workspace/stocks-ai-view/scripts/domain_lease.py --release <agent> <domain> --status done --output x.json --note "结论"
```

- `done` 是**永久锁定**（重复派会被拒并告知产出在哪），除非 `--force`
- 认领 24h 未完成自动 `expired`，可被重派
- 认领表文件：`/root/workspace/stocks-data-live/data/research/domain_lease.json`

### 2. 探测节流 `/root/workspace/stocks-ai-view/scripts/probe_throttle.py`
```bash
python3 /root/workspace/stocks-ai-view/scripts/probe_throttle.py --check tencent_quote   # 退出码 1 = 24h 内测过，跳过
python3 /root/workspace/stocks-ai-view/scripts/probe_throttle.py --record <id> --code 200 --ok 1 --agent <名>
```

- 已把 registry 里 68 个已验证源 seed 进 `/root/workspace/stocks-data-live/data/research/probe_log.json`

## 派子代理的 prompt 必须包含（5 条）
1. 只负责 `--claim` 过的那**一个域**
2. 「已完成的域不要再碰」+ 指向 `domain_lease.json`
3. 「已实测的源不要再测」+ 指向 `probe_log.json`
4. 只写自己那个产出文件，不要动 `scripts/`
5. **每探明 2-3 个接口就落盘**（App 进程可能被关闭导致成果全丢——已发生过两次）

## 派发时还要注意
- `writePaths` 要放开 `/tmp/<name>/` 给临时探测脚本，否则写租约会拦到子代理失败（已发生过两次 WRITE_LEASE_BLOCKED）
- 并行派发时 writePaths 不得相交

## 当前状态
14/14 域已完成（algo 无外部接口），68 个源已验证，产出都记在 `domain_lease.json` 里。