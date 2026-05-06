---
name: ip-asset-inventory-qcc
description: >
  知产资产清单 SKILL · 企查查 MCP V2.0 增强版。
  科技企业 IP 资产核查工具。V2.0 新增知产出质 + 历史专利/商标两层能力，识别已被质押融资或已退出的 IP。

  适用场景：法务 / 合规 / 律师 / 知产管理场景。

  使用方式：/ip-asset-inventory 企业名称 [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Legal Assistant Skills (Enhanced with QCC MCP V2.0)
  version: "2.0"
  plugin-commands: "/ip-asset-inventory"
  mcp-integrations: "qcc-company, qcc-risk, qcc-history, qcc-executive, qcc-operation, qcc-ipr"
---

# 知产资产清单 · 企查查 MCP V2.0 增强版

## SKILL 定位

科技企业 IP 资产核查工具。V2.0 新增知产出质 + 历史专利/商标两层能力，识别已被质押融资或已退出的 IP。

## 工作流维度

1. 专利清单（qcc-ipr get_patent_info）+ 发明占比
2. 商标清单（get_trademark_info）+ 核心分类
3. 软件著作权（get_software_copyright_info）
4. 作品著作权（get_copyright_work_info）
5. **V2.0 新工具：知产出质**（get_ipr_pledge —— 无形资产是否已抵押融资）
6. **V2.0 新能力：历史专利 / 商标**（qcc-history —— 已失效 / 已转让的 IP 轨迹）
7. 技术标准参与（get_standard_info —— 行业影响力）

## 评级

IP 资产评级 S/A/B/C



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
