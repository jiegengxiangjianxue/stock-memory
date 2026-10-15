---
name: stock_search
description: stocks-ai-view 前端搜索功能修复记录
---
## 问题发现与修复

### 1. symResults 容器定位问题 (index.html)
**现象**: 搜索下拉菜单显示在页面顶部 (0,0)，而非输入框下方。
**原因**: CSS 中 `.symResults` 没有设置 `top` 属性，仅设置了 `left`。
**修复**: 添加 `top: 100%;` 使其紧跟输入框下方显示。

### 2. search_index.json 生成脚本缺失
**现象**: `utils/make_search_index.py` 文件不存在，导致无法定期重建搜索索引。
**修复**: 创建 `utils/make_search_index.py`，从 `data/quotes.json` 读取最新报价数据生成 JSON 索引。

### 3. CI Workflow 更新
**现象**: `.github/workflows/update-quotes.yml` 未在 quotes 更新后重建 search_index。
**修复**: 在 workflow 中添加 step 调用 `make_search_index.py`。

### 4. search_index.json 重建
**现象**: 当前 `search_index.json` 可能数据过期或格式不一致。
**修复**: 运行新的生成脚本重建索引，确保包含 code, name, price, chg 字段。

## 修改文件列表
1. `frontend/index.html` - 修复 .symResults CSS 添加 `top: 100%`
2. `utils/make_search_index.py` - 新建生成脚本
3. `.github/workflows/update-quotes.yml` - 添加 search_index 重建步骤
4. `data/search_index.json` - 重新生成

## Commit Message
fix: 搜索功能修复 + search_index 确保定期重建