---
name: 担保方资信核查-guarantor-check-qcc
description: >
  担保方资信核查 SKILL · 企查查 MCP V2.0 增强版。
  贷款担保审批前对保证人（第三方企业）的实质偿付能力核查工具。与授信尽调不同，担保核查的核心问题是"当主债务人违约时，担保方能否实际承担代偿责任"——这要求对担保方的财务底盘、资产负担情况、历史履约记录、法代个人偿债力做集中深度的穿透评估。

  核心能力：
  - 担保方真实财务底盘：`get_financial_data` 评估净资产规模、负债结构、偿还能力比率
  - 担保占用情况：股权质押 / 股权出质 / 动产抵押 / 土地抵押 / 对外担保余额——识别担保方自身担保能力是否已被透支
  - 历史履约能力追溯：qcc-history 识别担保方过往是否发生过连带代偿失败事件
  - 法代个人偿债力：一旦担保方自身偿付不足，法代个人是否具备追偿可执行价值
  - 担保有效性评级 × 建议担保金额上限 × 增信措施清单

  适用场景：银行贷款担保审批 / 债券发行担保人核查 / 融资租赁担保方评估 / 保证合同签约前资信核查。

  使用方式：/guarantor-check 担保方企业名称 [--guarantee-amount 担保金额] [--guarantee-type 连带责任|一般保证] [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Vibe Lawyering / Banking (Enhanced with QCC MCP V2.0)
  version: "2.0"
  plugin-commands: "/guarantor-check"
  mcp-integrations: "qcc-company, qcc-risk, qcc-history, qcc-executive"
  industry: "Financial Services - Credit Guarantee / Surety"
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

# 担保方资信核查 · 企查查 MCP V2.0 增强版

## SKILL 定位

本 SKILL 服务于贷款担保审批、债券发行担保人核查、融资租赁担保方评估、保证合同签约前资信核查等场景的担保方深度穿透需求。与"授信尽调"评估"主债务人偿还贷款的能力"不同，"担保方资信核查"回答的是一个更严苛的问题——**当主债务人违约时，这家担保方能否实际履行代偿责任？**

这个问题的核心不仅是担保方自身的财务状况，更要看担保方**当前还有多少未被占用的担保能力**。一家账面净资产 10 亿元的担保方，如果已经对外担保 8 亿元、股权已质押 70%、土地已抵押殆尽——那它对新合同的实际兜底能力可能不到 1 亿元。V2.0 MCP 的升级让这种"担保额度透支识别"成为可能。

## MCP 依赖与配置

必选：
- `qcc-company`（企业基座）—— 基础工商 + `get_financial_data` 财务底盘
- `qcc-risk`（风控大脑）—— 核心依赖，全量担保类工具：股权出质 / 股权质押 / 动产抵押 / 土地抵押 / 对外担保 / 股权冻结

强烈建议：
- `qcc-history`（历史存档）—— 历史履约能力评估
- `qcc-executive`（人员画像）—— 担保方法代个人偿债力

## 通用执行原则

**第一，净资产不等于担保能力。** 担保方的理论担保上限是净资产，但实际可调度的担保额度要扣除"已被占用的部分"：已对外担保余额 + 已质押的股权对应净资产 + 已抵押的土地对应净值 + 有保证金用途的现金。SKILL 必须计算"剩余可用担保额度"。

**第二，担保关系的传递性风险必须识别。** 担保方自己也可能被其他方担保（互保圈），这种传递性关系的一旦爆雷将同时击穿多个担保链。对对外担保超过净资产 30% 的担保方，须特别标注"互保传染风险"。

**第三，连带责任与一般保证必须区分评级。** 同一担保方在"连带责任保证"下的风险暴露远高于"一般保证"。SKILL 评级需按担保类型做差异化输出。

**第四，关联担保与非关联担保必须区分。** 担保方与主债务人如为关联企业（同一实控人 / 股东重叠 / 法代重叠），担保的有效性需额外打折——因为"一荣俱荣、一损俱损"。

**第五，担保期限与担保方资质证书有效期必须对齐。** 如担保方部分资产有期限（如土地使用权 20 年），担保期限若超过资产剩余年限，相应部分需做折价。

## 工作流

### 维度一：担保方基本信息 × 财务底盘

工具链：
- `mcp__qcc-company__get_company_registration_info` — 工商登记
- `mcp__qcc-company__get_actual_controller` — 实际控制人
- `mcp__qcc-company__get_shareholder_info` — 股东结构
- `mcp__qcc-company__get_financial_data` — **3 年完整财报**（核心）
- `mcp__qcc-company__get_external_investments` — 对外投资（判断资产分布）

核心担保能力指标（基于 `get_financial_data`）：

| 指标 | 计算方法 | 担保能力意义 |
|------|---------|------------|
| 净资产 | 所有者权益总计 | 理论担保上限 |
| 速动资产 | 流动资产 - 存货 | 紧急兑现能力 |
| 货币资金 | 从资产负债表直接取 | 立即可用现金 |
| 资产负债率 | 负债合计 / 资产合计 | 担保能力被负债侵蚀程度 |
| 有息负债 | 负债合计 - 应付账款等营运负债（估算） | 真实偿债压力 |
| 对外担保 / 净资产 | `get_guarantee_info` 汇总 / 净资产 | 担保额度透支率 |

### 维度二：担保占用情况（核心维度）

工具链：
- `mcp__qcc-risk__get_equity_pledge_info` — 股权出质（非上市公司股权）
- `mcp__qcc-risk__get_stock_pledge_info` — 股票质押（上市公司股东）
- `mcp__qcc-risk__get_chattel_mortgage_info` — 动产抵押（设备、车辆、存货）
- `mcp__qcc-risk__get_land_mortgage_info` — 土地抵押
- `mcp__qcc-risk__get_guarantee_info` — 对外担保明细
- `mcp__qcc-risk__get_equity_freeze` — 股权冻结（已被司法冻结的股权完全不能作为新担保）
- `mcp__qcc-ipr__get_ipr_pledge` — 知识产权出质（V2.0 新工具）

分析要点：

**剩余可用担保额度 = 净资产 - 已质押股权对应净资产 - 已抵押资产价值 - 已对外担保余额 - 已冻结股权对应净资产**

- 若"剩余可用担保额度 < 拟担保金额 × 1.5"，担保有效性存疑，评级至少下调一级
- 若担保方净资产为负（资不抵债），任何担保承诺均为"形式担保"，直接触发 D 级
- 动产抵押和土地抵押的评估值通常应打 70% 折扣后作为可变现值

### 维度三：司法风险与历史履约能力

工具链（当前层）：
- `mcp__qcc-risk__get_dishonest_info` / `get_judgment_debtor_info` / `get_high_consumption_restriction`
- `mcp__qcc-risk__get_case_filing_info` / `get_judicial_documents`

工具链（历史层）：
- `mcp__qcc-history__get_historical_dishonest` —— 识别过往失信事件
- `mcp__qcc-history__get_historical_judgment_debtor` —— 历史被执行
- `mcp__qcc-history__get_historical_terminated_cases` —— 历史终本
- `mcp__qcc-history__get_historical_admin_penalty` —— 历史行政处罚

分析要点：

担保方自身如存在以下任一情况，担保有效性直接触发重大质疑：
- 当前失信被执行——已失去履约资格
- 当前股权冻结——现有资产已被先行债权人封锁
- 历史曾有 3 次以上被执行——履约意愿存疑
- 近 3 年有 1 次以上已履行的失信（"修复型"）——评级下调半级至一级

如担保方本身就是担保纠纷的被告（检查裁判文书中的案由），需直接标注"连带代偿历史纠纷"，作为评级关键依据。

### 维度四：法代与实控人个人偿债力

**【个人风险先扫后钻 · 2026-06-08 · 对齐 A 层铁律 5 个人维度】** 对每位目标人（法代/实控人/董监高），**先调 `mcp__qcc-executive__get_executive_risk_scan`（searchKey=企业完整名/USCC + personName=姓名，双锚定）一次返回其 18 项个人风险维度命中计数 → 仅对 count>0 维度下钻下列对应 `get_executive_*` 原子工具取明细**；count=0 跳过。❌ 禁止不先扫、逐个散弹枪调个人风险原子。单人工具：多人则逐人各扫一次，不对全体董监高自动循环。
工具链：
- `mcp__qcc-executive__get_executive_dishonest` / `get_executive_judgment_debtor` / `get_executive_high_consumption_ban` / `get_executive_exit_restriction` — 个人当前风险
- `mcp__qcc-executive__get_executive_controlled_companies` / `get_executive_investments` — 个人其他资产
- `mcp__qcc-executive__get_executive_historical_dishonest` — 个人历史失信

分析要点：

担保方企业层偿付不足时，是否可刺破公司面纱追究法代 / 实控人责任？这取决于：
- 法代 / 实控人是否签署了个人连带担保条款
- 法代 / 实控人本人是否具备可执行资产
- 法代 / 实控人是否有跑路风险（限制出境 = 已被其他债权人盯上）

如担保方法代同时为主债务人的法代 / 实控人，本条维度的权重下降——关联担保本身不提供额外兜底，仅是"形式增信"。

### 维度五：综合评级 × 建议担保金额 × 增信措施

#### 评级体系（A/B/C/D 四级）

| 评级 | 核心标准 | 担保建议 |
|------|---------|---------|
| **A 级** | 净资产充沛 + 无当前担保占用或占用率 < 30% + 无司法风险 + 实控人清洁 | 可接受连带责任担保，担保上限为净资产的 50% |
| **B 级** | 净资产良好 + 担保占用 30-50% + 有已履行历史事件 + 实控人清洁 | 可接受担保但需附加反担保或保证金，上限为净资产的 30% |
| **C 级** | 担保占用 50-80% 或 近 3 年有已修复的失信 或 实控人历史已修复事件 | 担保有效性打问号，需换人或补充抵押，上限为净资产的 10% |
| **D 级** | 担保占用 > 80% 或 当前失信 / 股权冻结 / 实控人出险 或 资不抵债 | **不建议接受担保**，或仅作为形式担保不计入风险缓释 |

#### 增信措施建议

- A 级：标准连带责任保证合同即可
- B 级：要求反担保 + 实控人个人连带责任 + 交叉违约条款
- C 级：要求追加土地抵押 / 应收账款质押 + 要求担保方每季报送财报
- D 级：建议放弃该担保方，重选新担保主体

## 输出模板

- 章节 1：**执行摘要 · Decision Pack**（评级 + 剩余可用担保额度 + 关键风险 + T+0/T+3/T+7 Action）
- 章节 2：数据来源与互证方法
- 章节 3：担保方基本信息 × 财务底盘（6 项核心担保能力指标）
- 章节 4：**担保占用情况**（股权质押 / 股权出质 / 动产抵押 / 土地抵押 / 对外担保 / 冻结——详细清单）
- 章节 5：司法风险与历史履约能力
- 章节 6：法代与实控人个人偿债力
- 章节 7：综合评级 × 建议担保金额 × 增信措施
- 章节 8：数据来源、采集时间戳、免责声明

## 参数

- `--guarantee-amount <金额>`：拟担保金额（必填）
- `--guarantee-type <类型>`：担保类型（连带责任 / 一般保证 / 物保）
- `--related <true|false>`：是否关联担保（默认 false；关联担保将自动降级评估）
- `--format md|docx|pptx`：输出格式，默认 md

## 边界与免责

本 SKILL 基于担保方企业主体侧数据评估，不涉及主债务人与担保方的合同条款实质审查（如担保范围、期限、放弃先诉抗辩权等）——这些属于律师审合同的工作范围。

担保方对外担保明细 (`get_guarantee_info`) 可能存在披露不完整（如民间担保、隐性担保），特别是对大型集团企业的互保圈情况，建议配合征信系统做交叉验证。

担保决策的最终判断应由信贷审批委员会结合业务关系、历史合作记录、宏观风险等综合判断，本 SKILL 输出仅为决策支持材料。

---

**SKILL 版本**：v2.0（MCP V2.0 升级版）
**适配 MCP 版本**：146 工具 / 6 Server 全量版
**所需 Server**：qcc-company（必选）、qcc-risk（必选）、qcc-history（建议）、qcc-executive（建议）
