---
name: ip-infringement-alert-qcc
description: >
  知产侵权预警 SKILL · 企查查 MCP V2.0 增强版。
  产品上市、合作签约或新业务推广前的知识产权侵权风险预警工具，输出"🔴 高侵权风险 / 🟡 中 / 🟢 低"三档评级。

  核心能力：
  - **目标企业 IP 组合扫描**（`mcp__qcc-ipr__get_patent_info` + `get_trademark_info` + `get_software_copyright_info` + `get_copyright_work_info`）—— 完整 IP 资产清单
  - **商标近似检索**（含文字 + 图形）—— 与本方拟用商标做相似度比对，识别冲突风险
  - **专利权利要求覆盖范围分析**（`get_patent_info` + 权要解析）—— 评估目标专利对本方产品的覆盖深度
  - **V2.0 历史商标相似挖掘**（`mcp__qcc-history__get_historical_trademark`）—— 把搜索范围扩大到已失效 / 已转让的历史商标，避免后悔型侵权
  - **IP 诉讼历史**（`mcp__qcc-risk__get_judicial_documents` 案由按"侵害商标权 / 侵害专利权 / 侵害著作权"过滤）—— 识别"诉讼狂"型权利人
  - 知产质押状态（`mcp__qcc-ipr__get_ipr_pledge`）—— 已质押 IP 的复杂权属关系

  适用场景：新产品上市前 IP 风险预审 / 合作签约前知产尽调 / 品牌推广前商标冲突排查 / 法务团队主动排查规避措施。

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
