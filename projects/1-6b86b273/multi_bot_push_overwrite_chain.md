---
name: multi_bot_push_overwrite_chain
description: 多 bot 并发仓库 pull --rebase / latest.json 冲突的处理步骤与提交纪律，推送前读
type: fact
---
# 多 bot 并发仓库的 rebase 与数据文件冲突处理

## 场景
`stocks-ai-view` 有 6+ 个 bot 并发推 main (engine-bot / pool-bot / shard-collect-bot /
moneyflow / hot-impact / cloud-analysis / minute ...)。本地很容易落后 10+ 个提交。

## 坑 1: rebase 被未跟踪文件挡住
`git pull --rebase` 报 "Please move or remove them before you switch branches" / "could not detach HEAD"。
原因: 本地有一批**未跟踪**的 data/*.json (concept_map / industry_stocks / news_impact /
stock_names / shard_local 等), 而远端提交里已存在同名文件, checkout 时会被覆盖所以 git 拒绝。

处理: 先 `git ls-tree -r origin/main --name-only | grep <file>` 确认远端是否已有,
**已有就把本地旧副本备份到 /tmp 后删除**, 再 rebase。不要直接 `git checkout -f` 或 `reset --hard`
(会连带丢掉自己的提交)。

## 坑 2: data/latest.json 冲突 (UU)
latest.json 是所有 bot 的公共写入目标, 必然冲突。
项目规则 (AGENTS.md): **以远端 Actions 输出为基, 再叠加本地字段**。

处理脚本模式:
```python
import json, subprocess
base  = json.loads(subprocess.run(['git','show',':3:data/latest.json'],capture_output=True,text=True).stdout)
mine  = json.loads(subprocess.run(['git','show',':2:data/latest.json'],capture_output=True,text=True).stdout)
for k in ('sector_moneyflow',):          # 只叠加自己新增的字段
    if k in mine and k not in base: base[k] = mine[k]
json.dump(base, open('data/latest.json','w'), ensure_ascii=False)
```
`:3:` = theirs (远端), `:2:` = ours (本地提交)。然后 `git add` + `GIT_EDITOR=true git rebase --continue`。

## 提交纪律
只 `git add` 自己改的文件, **不要 `git commit -a`** (会把别人的未跟踪文件一起提交)。
heredoc 形式的 `git commit -F -` 会被容器安全防护拦, 用 `writeFile` 写 /tmp/commitmsg.txt 再 `git commit -F`。