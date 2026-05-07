---
name: labor-compliance-qcc
description: >
  劳动合规风险排查 SKILL · 企查查 MCP V2.0 增强版。
  企业用工合规系统性排查工具，输出"合规 A/B/C/D"四档评级，对应"标准准入 / 加强监测 / 附条件 / 拒绝"四种处置。

  核心能力：
  - **劳动仲裁与诉讼扫描**（`mcp__qcc-risk__get_service_announcement` 劳动仲裁过滤 + `get_judicial_documents` 案由过滤）—— 识别劳动争议密度
  - **社保 / 公积金合规**（结合招股书披露与 `mcp__qcc-company__get_annual_reports` 从业人数核对）—— 核实缴纳率与未缴原因
  - **税务违法 / 欠税扫描**（`mcp__qcc-risk__get_tax_violation` / `get_tax_arrears_notice`）—— 间接反映用工成本合规
  - 行政处罚（劳动类）（`mcp__qcc-risk__get_administrative_penalty`）—— 人社部门、劳动监察大队的处罚记录
  - **V2.0 历史劳动仲裁追溯**（`mcp__qcc-history__get_historical_service_notice` / `get_historical_judicial_docs`）—— 识别"连年劳动纠纷型"企业
  - **IPO 前劳动专项审查**（招聘速率 × 社保合规 × 工时灰区）—— 对接证监会问询要点
  - 招聘活跃度推演（`mcp__qcc-operation__get_recruitment_info`）—— 月度招聘速率 + 人力成本测算

  适用场景：并购尽调 / 投前 DD / 供应商合规审查 / 新客户准入评估 / IPO 申报合规底稿。

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
