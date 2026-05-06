---
name: labor-compliance-qcc
description: >
  劳动合规风险排查 SKILL · 企查查 MCP V2.0 增强版。
  企业用工合规系统性排查工具。V2.0 新增历史劳动仲裁追溯能力。

  适用场景：法务 / 合规 / 律师 / 知产管理场景。

  使用方式：/labor-compliance 企业名称 [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Legal Assistant Skills (Enhanced with QCC MCP V2.0)
  version: "2.0"
  plugin-commands: "/labor-compliance"
  mcp-integrations: "qcc-company, qcc-risk, qcc-history, qcc-executive, qcc-operation, qcc-ipr"
---

# 劳动合规风险排查 · 企查查 MCP V2.0 增强版

## SKILL 定位

企业用工合规系统性排查工具。V2.0 新增历史劳动仲裁追溯能力。

## 工作流维度

1. 劳动仲裁与诉讼（qcc-risk get_service_announcement 劳动仲裁）
2. 社保欠缴 / 税务违法（get_tax_arrears / get_tax_violation）
3. 行政处罚（劳动类）
4. **V2.0 新能力：历史劳动仲裁**（qcc-history —— 识别连年劳动纠纷型企业）
5. IPO 前劳动专项审查（招聘规模 × 社保合规）

## 评级

合规 A/B/C/D



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
