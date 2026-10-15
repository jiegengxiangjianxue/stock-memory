---
name: user_prefers_subprocess_over_requests
description: 用户偏好用 subprocess 而非 requests 操作 Git
type: preference
---
用户在与 Git 交互时偏好使用 Python `subprocess` 调用 git 命令行，而不是使用 `requests` 库，因为目标环境可能未安装 requests。