---
name: readiness-collection
description: 工厂AI项目六维准备度资料收集指引（阶段2）。当用户拿到场景预诊断清单后，需要知道每个维度（战略/数据/技术/工艺/组织/变革）具体收集什么内容、飞书文档如何命名和写作时使用。完成收集后衔接准备度诊断。
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc Read Write
---

# 准备度资料收集（阶段 2）

**前提：** 已完成场景预诊断，获得了定制资料收集清单。

**核心原则：**
1. 按 Step 0 给出的定制清单填写，每维一份飞书文档
2. 写在飞书里并粘贴链接，不要只在对话里回文字
3. 写"没有"比不写好——某项资料不存在时说明"目前无，原因是……"
4. 首次不完整没关系——先填现有，空缺后续补充

---

## 🧭 内联问卷生成协议（本 skill 通用执行块）

本 skill 所有"问卷式输入点"**直接内联**调用 `deep_research` + `sunny_ai__feishu_create_doc`——**AI 主动为每维生成模板飞书文档**给用户填，而非让用户自己新建空文档。

**A · deep_research（必做）** — query 的目标是**"向用户提问所用的访谈/评估方法论"**（SIPOC / GQM / 5W1H / A3 / CRISP-DM Business Understanding / Bernard Marr AI Use Case Canvas / NIST AI RMF Map / Hoshin X-Matrix / AIAG FMEA / Prosci ADKAR / Kotter 8-step / McKinsey Rewired Diagnostic 等），**不是**"问题的领域知识答案"。用户此刻还没给出完整问题，我们要的是"**怎么问才能把信息问全**"的访谈工具，不是"问题怎么解决"的答案。

query 构造：`[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议" + [intake goal 英文] + "intake framework / interview protocol / structured questionnaire / assessment checklist" + 列举 2-3 个真实方法论名`

权威来源：McKinsey Rewired / BCG AI at Scale / WEF Lighthouse / INCIT AIMRI / ISO 13053 / 55001 / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter / Basili GQM。

**B · 合成 3-5 条洞察**。
**C · 生成飞书** — 🟡/⚪；6:4；量化 ≥ 50%；每章节 ≤ 8 行；**必含评级判据表**（1-5 级）；末尾「📌 填写说明」。
**D · 调 `sunny_ai__feishu_create_doc`**（失败降级为贴 markdown）。
**E · 回用户**：来源 + 链接 + 要点 3 条（本维要点、截图优先、估算请标注）。
**F · 等贴回** → 解析 → 追问缺失 🟡 → 低置信度继续。

---

## 起步询问

不要一次性列出全部 6 维。先问：

> 从哪个维度开始？建议先做 **②数据准备度** 和 **④工艺可控度**——这两个是 AI 启动的关键门槛。

用户选定后，为该维度执行内联问卷生成协议（差异参数见下）。

---

## ① 战略价值度——执行内联问卷生成协议

**评估什么：** AI 项目值不值得做？目标是否量化？管理层是真支持还是挂名？ROI 是否经得起追问？

**框架：** McKinsey Rewired "CEO Elevator Test"、BCG "AI at Scale" 真实承诺、Gartner AI 价值可行性矩阵

差异参数：

```
research_query: "战略价值度深度评估访谈协议 McKinsey CEO elevator test 4-section canvas BCG strategic value detailed interview Gartner AI business case structured questionnaire business loss quantification sponsor commitment ROI stress test intake 1-5 级评级访谈"
topic: "工厂 AI 项目战略价值度 · D1 问题量化 / D2 战略对齐 / D3 领导承诺 / D4 ROI 合理性"
scenario: [用户工序 + 痛点]
industry: [用户行业]
doc_name: "[厂区]-[项目简称]-战略价值度-[YYYYMMDD]"
purpose: "量化年化业务价值，确认战略对齐、管理层承诺真实度与 ROI 压力测试"
```

**问卷章节必含（4 组信号）：**
- **A 问题量化（AI 电梯测试）**：一句话业务问题（🟡）+ 定量数据表（缺陷类型/频率/单件损失/年化，🟡 4 行数字）
- **B 战略对齐**：公司 Top 3 战略目标（🟡）+ 本项目连接（🟡）+ 不做的 6/12 月情景（🟡）
- **C 领导承诺**：Owner 姓名+职级（🟡）+ 预算审批权（🟡）+ 绩效挂钩（🟡）+ 专项预算金额（🟡）+ 承诺程度口头/书面（⚪）
- **D ROI 压力测试**：三情景（60%/100%/120%）年化价值 + 总投入 + 回收期 + 是否正（🟡 表格）

**AI 评级判据（1-5 级，内含于文档）：**
- 1 级：模糊；无量化基线；挂名 Owner；无预算
- 2 级：有方向无数字；Owner 挂名；预算"正在申请"
- 3 级：问题量化清楚；Owner 有实权；有预算；ROI 初步合理
- 4 级：连公司战略；Owner 绩效挂钩；有止损；保守 ROI 正
- 5 级：项目入公司 OKR；成本收益系统追踪；量化不行动代价

话术：

> 这是战略价值度。重点填好业务损失的**量化数字**——决定整个项目的优先级。
> McKinsey：无法通过"电梯测试"说清楚的项目，失败率是清晰项目的 3 倍。

---

## ② 数据准备度——执行内联问卷生成协议

**评估什么：** D1 质量 / D2 X-Y 追溯 / D3 数据量 / D4 标注 / D5 稳定性 / D6 治理。

**最低启动门槛 ≥ 3 级**（行业研究：60-85% 制造 AI 失败源于数据准备不足）

**框架：** ISO/IEC 25012、DAMA-DMBOK、NIST AI RMF 1.0、Google ML Rules

差异参数：

```
research_query: "数据准备度 D1-D6 深度评估访谈协议 Google ML Rules detailed checklist NIST AI RMF Map questions AWS ML Lens data pillar questionnaire DAMA DMBOK deep dive interview ISO 25012 assessment matrix 六子维度结构化访谈 1-5 级评级"
topic: "工厂 AI 项目数据准备度 · D1-D6 六子维度"
scenario: [AI 类型：视觉/参数优化/预测性维护 + 工序]
industry: [用户行业]
doc_name: "[厂区]-[项目简称]-数据准备度-[YYYYMMDD]"
purpose: "评估该工序数据是否满足 AI 训练部署要求，识别瓶颈"
```

**问卷章节必含（D1-D6 六子维度）：**
- **D1 质量**：缺失率（🟡 数字）/ MSA 或 GR&R 报告（🟡 截图插入）/ 异常值处理（⚪）
- **D2 X-Y 追溯**：X 字段清单（🟡）/ 关联方式（🟡 字段名）/ 关联率（🟡 数字）
- **D3 数据量**：记录数（🟡）/ 时间跨度（🟡）/ 缺陷事件次数（🟡）/ 截图可贴（⚪）。门槛参考：视觉 ≥ 1,000-5,000 张/类；参数优化 ≥ 100 组合且 ≥ 6-12 月；预测维护 ≥ 2-5 年且 ≥ 30-50 次失效
- **D4 标注**：标注者数量（🟡）/ 标注规范是否有（🟡）/ 一致性测试 Cohen's Kappa（⚪）
- **D5 稳定性**：近 3 月 vs 6 月前分布变化（🟡）/ 换料/大修/参数调整日志（⚪ 截图）
- **D6 治理**：数据字典（⚪ 附件）/ AIE 访问权限（🟡）/ 访问延迟（🟡）

话术：

> 这是数据准备度——AI 启动最关键门槛。不完整也能继续，能填多少填多少。
> GR&R 报告、数据统计截图**直接拖入飞书**，比文字更有价值。

---

## ③ 技术互联度——执行内联问卷生成协议

**评估什么：** 设备是否有数字接口、系统间数据流、AI 能否实时取数、网络安全。

**框架：** IIoT / OPC-UA / MQTT 工业协议、WEF 灯塔数字基础层、INCIT AIMRI 技术互联维度、Purdue 模型

差异参数：

```
research_query: "技术互联度深度评估访谈协议 IT OT 系统详细盘点问卷 Purdue Model detailed questions NIST CSF IT-OT assessment TOGAF architecture review questionnaire ISA-95 level interview 系统接口网络隔离 5 级成熟度评估访谈"
topic: "工厂 AI 项目技术互联度 · 系统 / 接口 / 网络 / 实时性 / 安全"
scenario: [工序 + 系统现状 + 网络规划]
industry: [用户行业]
doc_name: "[厂区]-[项目简称]-技术互联度-[YYYYMMDD]"
purpose: "评估现有 IT/OT 是否支持 AI 接入，识别改造需求"
```

**问卷章节必含：**
- 现有系统清单（🟡 MES/SCADA/QMS/ERP/PLC 名称 + 功能 + 数据内容 + 更新频率）
- 系统间数据流向（🟡 从产生到可用经过哪些系统 / 有无孤岛）
- 设备接口（🟡 OPC-UA/MQTT/RS485 等）
- 数据实时性（🟡 延迟数字）
- 网络与安全（🟡 IT/OT 隔离 / AI 访问审批）
- 等保级别（⚪）

---

## ④ 工艺可控度——执行内联问卷生成协议

**评估什么：** 参数是否可识别 / 可测量 / 可调整；MSA 是否可信；X→Y 机理是否理解。

**框架：** AIAG MSA（GR&R < 10% 优秀，10-30% 可接受，> 30% 需修复）、Six Sigma Cpk、FMEA

差异参数：

```
research_query: "工艺可控度深度评估访谈协议 Six Sigma DMAIC Measure deep interview AIAG FMEA detailed questionnaire ISO 22514 capability assessment 5M1E root cause interview MSA GR&R Cpk 工艺参数清单 X-Y 机理访谈 1-5 级评级"
topic: "工厂 AI 项目工艺可控度 · 参数 / MSA / Cpk / 机理 / 调整权限"
scenario: [工序 + 改善对象 + 工艺现状]
industry: [用户行业]
doc_name: "[厂区]-[项目简称]-工艺可控度-[YYYYMMDD]"
purpose: "评估工艺参数是否清晰可控，测量系统是否可信，调整权限是否开放"
```

**问卷章节必含：**
- 关键工艺参数清单（🟡 X 参数 + 当前设定范围 + 控制方式 + 修改权限）
- MSA / GR&R 状态（🟡 数值，报告截图插入）
- 工艺能力 Cpk（🟡 数值 + SPC 管控图状态 ⚪）
- X→Y 物理机理理解（🟡 基于理论 / 经验）
- 参数调整可行性（🟡 审批流程）
- 历史工艺变更记录（⚪）

---

## ⑤ 组织能力度——执行内联问卷生成协议

**评估什么：** AI/数据工程师是否存在、工艺知识可传递性、IT 支持、跨职能协作。

**框架：** McKinsey "T 型人才"、WEF 灯塔人才体系

差异参数：

```
research_query: "组织能力度深度评估访谈协议 McKinsey OHI detailed interview WEF Lighthouse talent lens questionnaire Prosci ADKAR capability deep dive Deloitte AI maturity assessment T-shape talent intake 人才 AI 经验 跨职能协作 1-5 级评级访谈"
topic: "工厂 AI 项目组织能力度 · 技术能力 / 知识传递 / IT 支持 / 协作"
scenario: [工厂规模 + 现有团队 + AI 基础]
industry: [用户行业]
doc_name: "[厂区]-[项目简称]-组织能力度-[YYYYMMDD]"
purpose: "评估内部 AI 人才、技术基础、学习意愿"
```

**问卷章节必含：**
- AI/数据技术能力（🟡 内部 / 外包 AIE 人数 + Python/ML 经验）
- 工艺知识可传递性（🟡 有文档 / 口耳相传）
- IT 技术支持（🟡 数据工程师可支持度）
- 跨职能协作历史（🟡 工艺+质量+IT 联合项目经验）
- 培训预算与意愿（⚪）
- 外部合作方（⚪）

---

## ⑥ 管理变革度——执行内联问卷生成协议

**评估什么：** 操作员/班长是否愿采纳、变更流程顺畅度、治理机制。

**框架：** McKinsey 变革管理研究（70% 规模化失败源于组织/人员），Kotter 8 步，ADKAR

差异参数：

```
research_query: "管理变革度深度评估访谈协议 Kotter 8 step detailed readiness questionnaire Prosci ADKAR stakeholder deep dive McKinsey Influence Model change survey 治理 预算 变革阻力 沟通机制 1-5 级评级访谈 manufacturing AI adoption deep intake"
topic: "工厂 AI 项目管理变革度 · 态度 / 流程 / 历史 / 治理"
scenario: [工厂层级 + 管理风格 + 变革历史]
industry: [用户行业]
doc_name: "[厂区]-[项目简称]-管理变革度-[YYYYMMDD]"
purpose: "评估管理成熟度和变革意愿，识别组织阻力"
```

**问卷章节必含：**
- 操作员 / 班长态度（🟡 开放 / 观望 / 抵制 + 原因）
- 变更管理流程（🟡 审批层数 + 提议到执行时间）
- 过往改善项目经验（🟡 顺利度 + 阻力 + 解决方式）
- 治理机制（🟡 有无专项委员会 + 决策层期望管理）
- 员工满意度历史（⚪）
- 过往变革成功率（⚪）

---

## 资料充分度自检

| 准备 | AI 能做 | 建议 |
|---|---|---|
| 六维都有具体描述 | 高置信度评分 + 可执行方法论和行动 | 理想，直接诊断 |
| 大部分有，少数空缺 | 中置信度 + 标注不确定维度 | 可启动，后续补 |
| 仅 ② 数据 + ④ 工艺有 | 最低可行诊断，重点判能否启动 | 这两个最关键 |
| 大部分信息不足 | 只能给初步方向 | 先回场景预诊断 |

---

## 完成后衔接

```
✅ 六维资料收集指引完成！

现在可提交诊断。请提供（或贴过来）：
1. 阶段 1 目标定义摘要（KPI 矩阵 + 1.3 校准 + 约束）
2. 六维飞书链接（或内容直接贴过来）

准备好了吗？
(1) 开始诊断
(2) 还需时间，稍后再来
```

- 选 1 / 直接贴资料 → Read `readiness-diagnosis/SKILL.md` 执行
- 选 2 → 告知命令 `/sunny-ai-project:readiness-diagnosis`
