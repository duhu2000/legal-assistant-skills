---
name: 合同相对方主体核验-contract-party-check-qcc
description: >
  合同相对方主体核验 SKILL · 企查查 MCP V2.0 增强版。
  合同签署前的主体快速核验工具，输出"A/B/C/D"四档评级，D 级直接拒绝签约。

  核心能力：
  - **工商登记三项一致性核验**（`mcp__qcc-company__verify_company_accuracy` + `get_company_registration_info`）—— 企业名 + USCC + 法代三项交叉匹配，登记状态（吊销 / 注销 / 异常）一票否决
  - **V2.0 历史工商变更追溯**（`mcp__qcc-history__get_historical_registration` + `get_historical_legal_rep`）—— 识别频繁变更注册地址 / 频繁更换法代的壳公司
  - **司法风险快扫**（失信 `get_dishonest_info` / 限高 `get_high_consumption_restriction` / 被执行 `get_judgment_debtor_info` / 股权冻结 `get_equity_freeze` / 经营异常 `get_business_exception`）—— 5 项核心红线
  - **V2.0 法代个人风险**（`mcp__qcc-executive__get_executive_dishonest` + `get_executive_high_consumption_ban` + `get_executive_exit_restriction`）—— 法代当前失信 / 限高 / 限出境直接触发签约风险
  - **经营活跃度辅助判定**（参保人数 + 招投标 + 招聘）—— 区分"形式存续 vs 实质经营"
  - 关联企业网络扫描（`mcp__qcc-executive__get_executive_controlled_companies`）—— 识别合同方背后真实集团关系

  适用场景：合同签署前主体快速核验 / 法务 / 合规 / 律师 / 商务签约前风控、新合作方准入审批。

  使用方式：/contract-party-check 企业名称 [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Legal Assistant Skills (Enhanced with QCC MCP V2.0)
  version: "2.0"
  plugin-commands: "/contract-party-check"
  mcp-integrations: "qcc-company, qcc-risk, qcc-history, qcc-executive, qcc-operation, qcc-ipr"
---

## 📖 QCC MCP 术语对照表（强制工具映射）

> **使用约定**：本表列出 SKILL 内业务简写与企查查 MCP 工具的精确映射。AI 执行本 SKILL 时遇到下表"业务简写"列的词汇，**必须调用对应"MCP 工具"列**，禁止使用 web search 或自由文本推测替代。完整规范见 [QCC-MCP-TERMINOLOGY.md](../../QCC-MCP-TERMINOLOGY.md)。

| 业务简写 | 规范全名 | 企查查 MCP 工具 |
| --- | --- | --- |
| 失信 | 失信被执行人 | `mcp__qcc-risk__get_dishonest_info` |
| 被执行 | 被执行人 / 判决债务人 | `mcp__qcc-risk__get_judgment_debtor_info` |
| 限高 | 限制高消费 | `mcp__qcc-risk__get_high_consumption_restriction` |
| 限出境 / 限境 | 限制出境 | `mcp__qcc-risk__get_exit_restriction` |
| 终本 | 终结本次执行案件 | `mcp__qcc-risk__get_terminated_cases` |
| 破产 / 重整 | 破产重整 | `mcp__qcc-risk__get_bankruptcy_reorganization` |
| 经营异常 | 经营异常 | `mcp__qcc-risk__get_business_exception` |
| 严重违法 | 严重违法失信 | `mcp__qcc-risk__get_serious_violation` |
| 行政处罚 / 重大处罚 | 行政处罚 | `mcp__qcc-risk__get_administrative_penalty` |
| 股权冻结 | 股权冻结 | `mcp__qcc-risk__get_equity_freeze` |
| 股权出质 | 股权出质 | `mcp__qcc-risk__get_equity_pledge_info` |
| 欠税 | 欠税公告 | `mcp__qcc-risk__get_tax_arrears_notice` |
| 税务异常 / 税务违法 | 税务异常 / 税收违法 | `mcp__qcc-risk__get_tax_abnormal` / `mcp__qcc-risk__get_tax_violation` |
| 受益所有人 / UBO | 受益所有人 | `mcp__qcc-company__get_beneficial_owners` |
| 实控人 / 实际控制人 | 实际控制人 | `mcp__qcc-company__get_actual_controller` |
| 主要人员 / 董监高 | 主要人员 | `mcp__qcc-company__get_key_personnel` |
| 抽查检查 / 双随机 | 双随机抽查 | `mcp__qcc-operation__get_random_check` |
| 吊销 | （登记状态字段判断）| 调 `mcp__qcc-company__get_company_registration_info` 取"登记状态" |
| 资不抵债 | （资产负债率字段判断）| 调 `mcp__qcc-company__get_financial_data` 判断负债率 > 100% |

---

# 合同相对方主体核验 · 企查查 MCP V2.0 增强版

## SKILL 定位

合同签署前的主体快速核验工具。V2.0 新增历史工商变更追溯 + 法代个人风险扫描两层能力。

## 工作流维度

1. 工商登记状态 + 三项一致性核验
2. **V2.0 新能力：历史工商变更**（qcc-history get_historical_registration/legal_rep —— 识别频繁变更的壳公司）
3. 司法风险快扫（失信 / 限高 / 被执行 / 股权冻结 / 经营异常）
4. **V2.0 新能力：法代个人风险**（qcc-executive —— 法代当前失信 / 限出境直接触发合同签署风险）
5. 经营活跃度辅助判定（参保 / 招投标）

## 评级

A/B/C/D 四级 · D 级拒绝签约



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
