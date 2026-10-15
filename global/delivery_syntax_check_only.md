---
name: delivery_syntax_check_only
description: 判断脚本/代码交付是否需要实际运行测试
---
用户多次明确要求：脚本写完后**语法验证通过即可交付**（如 `python3 -m py_compile` 或 `ast.parse`），**不需要实际运行或网络测试**。除非用户另行要求，不要自行发起网络请求验证。