# baostock 云端 workflow 挂死坑

db-min-0/1 的 minute5-backfill run 于 23:32 启动后 in_progress 9 小时无产出 (数据 tree 无增长)。
原因: baostock 长连接断线后 requests/bs 无超时, workflow 的 run step 永久挂起。

处理: cancel + redispatch (两仓均已重触发)。
规则: 云端 worker 内所有网络调用必须带 timeout= (baostock.login 后每次 query 也可能挂)。
若 run >1h 无数据增长即视为挂死。
