---
name: "python_escape_and_bom_fixes"
description: "遇到技能脚本的无效转义警告或 UTF-8 BOM 时读取。"
type: fact
---
已验证的修法：`optimized_main.py` 文档示例中的 Windows 路径反斜杠改为双反斜杠；`stock-market-pro/scripts/uw.py` 正则字符串中的 `\d` 改为 `\\d`；`sql-master/scripts/pipeline.py` 文件开头的 UTF-8 BOM 已移除。修复后全库 Python、JavaScript 和 Shell 语法检查均无错误。