---
name: license-validation-qcc
description: >
  合规资质有效性验证 SKILL · 企查查 MCP V2.0 增强版。
  企业合规资质的真实性与有效期核查工具。V2.0 新增历史行政许可追溯能力，识别已过期或已撤销的资质。

  适用场景：法务 / 合规 / 律师 / 知产管理场景。

  使用方式：/license-validation 企业名称 [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Legal Assistant Skills (Enhanced with QCC MCP V2.0)
  version: "2.0"
  plugin-commands: "/license-validation"
  mcp-integrations: "qcc-company, qcc-risk, qcc-history, qcc-executive, qcc-operation, qcc-ipr"
---

# 合规资质有效性验证 · 企查查 MCP V2.0 增强版

## SKILL 定位

企业合规资质的真实性与有效期核查工具。V2.0 新增历史行政许可追溯能力，识别已过期或已撤销的资质。

## 工作流维度

1. 资质证书现状（qcc-operation get_qualifications / get_administrative_license）
2. 电信业务许可（get_telecom_license）
3. **V2.0 新能力：历史行政许可**（qcc-history get_historical_admin_license —— 识别已过期资质）
4. 纳税信用 + 海关信用
5. 资质到期预警（12 个月内到期清单）

## 评级

✅ 合规 / ⚠️ 有到期风险 / ❌ 无效



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
