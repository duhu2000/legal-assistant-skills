---
name: ip-infringement-alert-qcc
description: >
  知产侵权预警 SKILL · 企查查 MCP V2.0 增强版。
  产品上市 / 合作签约前的 IP 侵权风险预警工具。V2.0 新增历史商标相似挖掘能力。

  适用场景：法务 / 合规 / 律师 / 知产管理场景。

  使用方式：/ip-infringement-alert 企业名称 [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Legal Assistant Skills (Enhanced with QCC MCP V2.0)
  version: "2.0"
  plugin-commands: "/ip-infringement-alert"
  mcp-integrations: "qcc-company, qcc-risk, qcc-history, qcc-executive, qcc-operation, qcc-ipr"
---

# 知产侵权预警 · 企查查 MCP V2.0 增强版

## SKILL 定位

产品上市 / 合作签约前的 IP 侵权风险预警工具。V2.0 新增历史商标相似挖掘能力。

## 工作流维度

1. 目标企业 IP 组合 vs 本方产品的覆盖范围对比
2. 商标近似检索（含文字 / 图形）
3. 专利权利要求覆盖范围分析
4. **V2.0 新能力：历史商标相似挖掘**（qcc-history get_historical_trademark —— 扩大相似商标搜索范围到历史记录）
5. IP 诉讼历史（get_judicial_documents 案由过滤）

## 评级

🔴 高侵权风险 / 🟡 中 / 🟢 低



## MCP 依赖

- 必选：qcc-company / qcc-risk
- V2.0 强烈建议：qcc-history（历史追溯）/ qcc-executive（法代画像）/ qcc-operation（经营活跃度）

## 输出模板

- 章节 1：Decision Pack（评级 + 关键判断 + 推荐 Action）
- 章节 2：数据来源
- 章节 3-6：各维度扫描结果（详见上文）
- 章节 7：V2.0 能力增量说明
- 章节 8：综合评级 × 处置建议

## 参数

- `--format md|docx|pptx`：输出格式，默认 md

## 边界与免责

本 SKILL 基于企查查 MCP V2.0 公开数据生成。特定法律场景（如商标近似性的最终判定 / 劳动仲裁的实体审查）需配合专业律师做实质审查。


**SKILL 版本**：v2.0 | **适配 MCP 版本**：146 工具 / 6 Server 全量
