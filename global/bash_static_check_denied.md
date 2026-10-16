---
name: "bash_static_check_denied"
description: "执行 shell 命令前先自查：避免命令替换、子 shell、绝对路径重定向等写法"
type: fact
---
沙箱 Bash 有静态安全校验：命令中出现**命令替换、子 shell、绝对路径重定向**等无法静态判定的构造时，会被判定为疑似破坏性操作并直接拒绝，返回：

```json
{"status":"error","code":"SYSTEM_DENIED","message":"安全防护：命令含命令替换、子 shell 或绝对路径重定向等无法静态判定的构造，且疑似破坏性操作，无法确认安全。如确需执行，可在调用时加 `elevate: true` 重试，系统将向用户请求授权。"}
```

应对：优先把命令改写成可静态判定、无嵌套替换的简单形式；确实必须执行时由报错提示的 `elevate: true` 路径重试，走用户授权流程。