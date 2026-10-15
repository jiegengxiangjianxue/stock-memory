---
name: incremental_disk_persist
description: 长任务/多模块任务需边执行边落盘，防止进程中断丢失
type: preference
---
用户要求长任务/多模块任务必须边执行边落盘：每完成一个模块立即写入目标文件，不要等全部完成后才保存，以防止 App 进程被关导致成果丢失。