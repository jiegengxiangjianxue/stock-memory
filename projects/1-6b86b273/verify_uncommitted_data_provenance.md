---
name: "verify_uncommitted_data_provenance"
description: "处理股票数据项目中的未提交文件时，先确认其来源和归属。 [待确认 2026-10-16，与 cloud_only_stock_data 冲突]"
type: workflow
pending: true
---
用户明确要求：不要假定现有未提交数据属于本次任务。修复、上传数据前应先核实这些文件的来源与归属，避免擅自覆盖或发布。