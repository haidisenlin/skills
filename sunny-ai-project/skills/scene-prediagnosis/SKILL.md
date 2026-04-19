---
name: scene-prediagnosis
description: "工厂AI改善场景预诊断（Step 0-A）。当用户已有具体目标工序或AI方向（如\"视觉检测\"、\"良率预测\"、\"参数优化\"、\"预测性维护\"），或刚完成场景发现，需要知道该准备哪些资料时使用。输出定制化六维资料收集清单。完成后衔接目标定义和准备度收集。"
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc Read Write
---

# 场景预诊断（Step 0-A）

---

## 🧭 内联问卷生成协议（本 skill 通用执行块）

> 本 skill 的所有"问卷式输入点"都**直接内联**调用 `deep_research` + `sunny_ai__feishu_create_doc`，**不经由 research-template**。每个输入点只提供差异参数（topic / scenario / 飞书命名 / 章节结构），其余动作按下列协议执行。

### 步骤 A · 调用 `deep_research`（必做，不可跳过）

按差异参数中给定的 `research_query` 执行一次 deep_research。

⚠️ **query 的目标是"向用户提问所用的访谈 / 评估方法论"**（SIPOC / GQM / 5W1H / A3 / CRISP-DM Business Understanding / Bernard Marr AI Use Case Canvas / NIST AI RMF Map / McKinsey Rewired Diagnostic / Hoshin X-Matrix / AIAG FMEA / Prosci ADKAR / Kotter 8-step 等结构化访谈协议），**不是**"问题的领域知识答案"。用户此刻刚开始，我们要的是"**怎么问才能把信息问全**"的工具，不是"问题怎么解决"的答案。

query 构造：

```
[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议"
+ 英文：[intake goal] + "intake framework / interview protocol / structured questionnaire / assessment checklist"
+ 列举 2-3 个真实方法论名
```

权威来源：McKinsey Rewired / BCG AI at Scale / WEF Lighthouse / INCIT AIMRI / ISO 13053 / 55001 / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter 8-step / Basili GQM / 工业互联网产业联盟（AII）。

### 步骤 B · 合成 3-5 条方法论洞察

- 该问题的标准评估维度/子项
- 每个子项最关键的量化指标
- 什么样的信息最能帮 AI 做高质量诊断
- 行业高质量模板的结构特征

### 步骤 C · 生成飞书文档内容（严格对齐标准）

- **字段分两档**：🟡 推荐填 / ⚪ 选填，**禁用"必填/必选/强制"**
- 推荐填:选填 ≈ **6:4**
- 量化字段占比 **≥ 50%**（数字/比例/计数优先于纯文字）
- 每个章节 **≤ 8 行**主要项目
- 每个字段都要有括号内填写说明（格式/范围/单位/示例）
- 每个章节给 1-2 行 *斜体示例*
- 末尾加「📌 填写说明」章节解释 🟡 和 ⚪ 的含义

文档骨架：

```
# [doc_name]

> 📋 用途：[一句话]
> 方法论来源：[2-3 个真实框架]
> 预计填写时间：约 [N] 分钟

## ① [章节]
| 字段 | 优先级 | 内容 | 填写说明 |
|---|---|---|---|
| ... | 🟡 推荐填 | | ... |
| ... | ⚪ 选填 | | ... |
| *示例* | — | *[示例数据]* | *（示例，请替换）* |

## 📌 填写说明
- 🟡 推荐填：缺了会影响 AI 输出质量，但不阻塞
- ⚪ 选填：加分项
```

### 步骤 D · 调用 `sunny_ai__feishu_create_doc` 创建飞书文档

```
title: [doc_name，按 "[厂区]-[项目简称]-[内容]-[YYYYMMDD]"]
content: [步骤 C 生成的 markdown 全文]
```

若调用失败，降级为直接把 markdown 贴给用户，让其手动建文档，并说明原因。

### 步骤 E · 回给用户的标准话术

```
━━━ 📚 方法论来源 ━━━
[2-3 行，基于 deep_research 结果]

━━━ ✅ 飞书文档已创建 ━━━
文档名称：[doc_name]
文档链接：[MCP 返回链接]

请打开文档，按模板填写后把链接粘贴回对话。

━━━ 💡 填写要点 ━━━
1. [最重要的填写注意事项]
2. [数据不确定时，估算可以，请标注"(估算)"]
3. [现场来不及填可留空，AI 会标注低置信度继续]
```

### 步骤 F · 等用户粘贴回来

用户贴回链接或内容后：
- 解析飞书文档内容
- 对明显缺失的 🟡 推荐填 字段做 1 轮追问（不阻塞）
- 标注低置信度继续下一步

---

## 第一轮：了解场景基本情况

如果用户来自场景发现（已有场景确认描述），直接跳到第二轮使用该描述。

否则，**执行内联问卷生成协议**，差异参数如下：

```
research_query: "制造业 AI 改善需求初次访谈框架 问题定义方法 SIPOC 5W1H Kepner-Tregoe A3 problem statement manufacturing AI project intake interview protocol CRISP-DM business understanding Bernard Marr AI use case canvas discovery structured questionnaire"
topic: "工厂 AI 改善场景预诊断——基本信息收集"
scenario: [如有来自 initiate 的路由信息则传入；否则为空]
industry: [用户行业，默认"精密光学/光电模组/光学仪器制造"]
doc_name: "[厂区代码]-[项目简称]-场景预诊断-基本信息-[YYYYMMDD]"
purpose: "快速了解用户想做什么 AI 改善、在哪个工序、最大痛点是什么，以便 AI 精准识别场景类型和六维收集优先级"
```

**问卷章节结构（最少 3 个核心字段）**：

| 字段 | 优先级 | 填写说明 |
|---|---|---|
| 改善领域（A-K 单选） | 🟡 推荐填 | A-生产效率 / B-质量管理 / C-设备可靠性 / D-精益流程 / E-供应链 / F-成本 / G-能源 / H-安全 / I-人才 / J-数字化 / K-自定义 |
| 具体工序 + 计划用的 AI 类型 | 🟡 推荐填 | 如"AOI 视觉检测 + 缺陷分类视觉 AI" |
| 该工序当前最大痛点 + 量化数字 | 🟡 推荐填 | 必含至少 1 个数字（漏检率/返工率/停机时长等） |
| 现有 KPI 基线值（月度） | ⚪ 选填 | 数字 + 单位 + 统计口径 |
| 管理层已关注该问题的程度 | ⚪ 选填 | 已立项 / 已提议 / 仅车间层讨论 |

**降级方案**（用户拒绝填文档）：退回对话模式，一次只问一个问题，按上面 3 个推荐填字段顺序逐个问。

**K 路径（自定义场景）**：用户选 K 时：
- 判断最接近哪类管理改善领域，给出最权威方法论（TQM/TPM/Six Sigma）
- 给出标准 KPI（公式+单位）、基线数据要求、世界级基准值
- 请用户确认方向后再进第二轮

---

## 第二轮：输出场景识别 + 启动飞书准备

基于第一轮信息，输出以下内容，然后**停下等用户确认**：

```
【场景识别结果】

AI 调优类型：[视觉 AI / 工艺参数优化 / 预测性维护 / 流程分析 / 混合型]

核心技术挑战：[2-3 句话]

最关键的准备度维度（按重要性排序）：
  第 1 位：[维度] — [为什么对本场景最关键]
  第 2 位：[维度] — [原因]
  第 3 位：[维度] — [原因]

---

📋 接下来逐步填写六维飞书文档。

⚠️ 所有信息请通过飞书云文档录入，不要只在对话里回复。
   原因：飞书文档可多人协作、长期积累、AI 重复引用——这是"文档飞轮"基础。

飞书统一命名：[厂区代码]-[项目简称]-[维度]-[YYYYMMDD]
例：YC-AOI视检-数据准备度-20260412

从【最关键维度】开始。回复"开始"继续。
```

---

## 第三轮起：逐维度引导（每次一个维度）

对每个维度**执行内联问卷生成协议**，差异参数见下方「维度参数表」。按第二轮识别的优先级顺序跑，跑完一个再跑下一个。

---

### 维度参数表

#### ① 战略价值度

```
research_query: "制造业 AI 项目战略价值度访谈问卷 业务价值提问框架 McKinsey CEO elevator test BCG strategic value canvas questions Gartner AI business case questionnaire sponsor commitment assessment interview protocol ROI stress test questions"
topic: "工厂 AI 项目战略价值度分析——年化业务价值量化与战略对齐"
scenario: [用户确认的工序名 + 改善目标]
doc_name: "[厂区]-[项目简称]-战略价值度-[YYYYMMDD]"
purpose: "量化本 AI 项目的年化业务价值，确认战略对齐与管理层承诺"
```

**问卷章节建议**：业务损失量化（不良品成本/停机损失/返工费用，🟡）/ 改善目标年化收益估算（🟡）/ 管理层承诺层级（🟡）/ 竞争对标（⚪）/ 战略规划引用（⚪）。

发给用户时附带话术：

> 这是六维收集的第 1/6 步——战略价值度。重点填好业务损失的量化数字——这决定整个项目的优先级。

---

#### ② 数据准备度

```
research_query: "制造业 AI 数据准备度评估访谈框架 结构化问卷 Google ML Rules checklist NIST AI RMF Map function questions AWS Well-Architected ML Lens data pillar questionnaire DAMA DMBOK interview guide ISO 25012 assessment categories data readiness intake protocol"
topic: "工厂 AI 项目数据准备度评估——样本量 / 质量 / 追溯 / 标注"
scenario: [AI 类型：视觉/参数优化/预测性维护，工序名]
doc_name: "[厂区]-[项目简称]-数据准备度-[YYYYMMDD]"
purpose: "评估该工序数据是否满足 AI 训练与部署要求，识别数据瓶颈"
```

**问卷章节建议**：数据源清单（🟡）/ 样本量与时间跨度（🟡）/ 数据质量（缺失率/异常率/🟡）/ 标注规范（🟡）/ 数据追溯与血缘（⚪）/ 数据治理责任人（⚪）。

话术：

> 第 2/6 步——数据准备度。数据不完整也能继续——能填多少填多少，AI 会基于现有信息判断可行性。

---

#### ③ 技术互联度

```
research_query: "工厂 IT OT 系统清单访谈框架 技术互联度评估问卷 Purdue Model structured questions NIST CSF IT-OT convergence assessment TOGAF baseline architecture questionnaire ISA-95 level interview protocol manufacturing AI deployment readiness intake"
topic: "工厂 AI 项目技术互联度评估——系统清单 / 接口 / 网络 / 部署环境"
scenario: [工序 + 系统现状 + 网络规划]
doc_name: "[厂区]-[项目简称]-技术互联度-[YYYYMMDD]"
purpose: "评估现有 IT/OT 基础设施是否支持 AI 接入，识别改造需求"
```

**问卷章节建议**：现有系统清单（MES/ERP/SCADA/PLC，🟡）/ 数据接口与协议（🟡）/ 网络架构与隔离等级（🟡）/ 计算资源（🟡）/ 云/边/端部署偏好（⚪）/ 信息安全等保级别（⚪）。

话术：

> 第 3/6 步——技术互联度。这些因素直接影响 AI 部署策略（云端 vs 边缘）。

---

#### ④ 工艺可控度

```
research_query: "工艺可控度评估访谈框架 Six Sigma DMAIC Measure phase interview AIAG FMEA structured questionnaire ISO 22514 process capability assessment protocol 5M1E diagnostic questions MSA GR&R intake checklist manufacturing process controllability interview"
topic: "工厂 AI 项目工艺可控度评估——关键参数 / 测量系统 / 控制权限 / 稳定性"
scenario: [工序 + 改善对象 + 工艺现状]
doc_name: "[厂区]-[项目简称]-工艺可控度-[YYYYMMDD]"
purpose: "评估工艺参数是否清晰可控，测量系统是否可信，调整权限是否开放"
```

**问卷章节建议**：关键工艺参数清单（🟡）/ 测量系统 MSA 状态（🟡）/ 工艺能力 Cpk（🟡）/ 工艺调整权限矩阵（🟡）/ SPC/控制图现状（⚪）/ 历史工艺变更记录（⚪）。

话术：

> 第 4/6 步——工艺可控度。这些是 AI 能否真正"指导操作"的关键。

---

#### ⑤ 组织能力度

```
research_query: "组织 AI 能力度评估访谈框架 McKinsey OHI organizational health index interview WEF Lighthouse talent lens questionnaire Prosci ADKAR capability readiness intake Deloitte AI maturity assessment 人才盘点结构化问卷"
topic: "工厂 AI 项目组织能力度评估——人才 / AI 经验 / 工具 / 合作 / 培训意愿"
scenario: [工厂规模 + 现有团队 + AI 基础]
doc_name: "[厂区]-[项目简称]-组织能力度-[YYYYMMDD]"
purpose: "评估组织内部 AI 人才、技术基础、学习意愿，判断项目能否顺利落地"
```

**问卷章节建议**：AI/数据相关人员数量与级别（🟡）/ 既往 AI 项目经验（🟡）/ 可用工具与平台（🟡）/ 外部合作方（⚪）/ 培训预算与意愿（⚪）/ 跨部门协作机制（⚪）。

话术：

> 第 5/6 步——组织能力度。很多 AI 项目失败不是技术问题，是团队准备不足。

---

#### ⑥ 管理变革度

```
research_query: "管理变革度 AI 采纳意愿评估访谈框架 Kotter 8 step readiness questionnaire Prosci ADKAR stakeholder intake interview McKinsey Influence Model change readiness survey governance sponsorship assessment protocol manufacturing AI adoption intake"
topic: "工厂 AI 项目管理变革度评估——治理 / 预算 / 变革阻力 / 沟通机制"
scenario: [工厂层级 + 管理风格 + 变革历史]
doc_name: "[厂区]-[项目简称]-管理变革度-[YYYYMMDD]"
purpose: "评估组织管理成熟度和变革意愿，识别组织阻力"
```

**问卷章节建议**：项目 Sponsor 层级与承诺（🟡）/ 预算来源与额度（🟡）/ 预期受影响岗位与阻力评估（🟡）/ 沟通机制（🟡）/ 过往变革成功率（⚪）/ 员工满意度历史（⚪）。

话术：

> 第 6/6 步——管理变革度。这一步最容易被忽视，但很重要。

---

## 收集进度跟踪（内部使用）

每完成一维后更新进度。引导下一维前先告知：「已完成 N/6，接下来是第 N+1：[维度名]」。

---

## 全部六维收集完毕后：输出完整汇总

**时机**：用户提交完第 6 维后（或用户说"先到这里，帮我汇总"）。

输出：

```
【六维资料收集汇总】
工序/场景：[场景名]
完成日期：[YYYYMMDD]

━━━ 一、资料完整度一览 ━━━

| 维度 | 飞书文档 | 关键信息完整度 | 加分项 |
|---|---|---|---|
| ①战略价值度 | [已提交/待补充] | [高/中/低] | [有/无] |
| ②数据准备度 | ... | | |
| ③技术互联度 | ... | | |
| ④工艺可控度 | ... | | |
| ⑤组织能力度 | ... | | |
| ⑥管理变革度 | ... | | |

━━━ 二、重点观察 ━━━
（2-3 个最值得在诊断阶段深入评估的地方）

━━━ 三、最小启动包确认 ━━━
可立即提交诊断的维度：[列举]
建议补充的维度：[列举 + 最关键缺口]

━━━ 四、下一步 ━━━
✅ 六维资料收集完成！

建议先完成【目标定义】确定 KPI 目标和基线，再提交六维做诊断。
回复"是"/"继续"进入 goal-definition，或说"跳过目标定义"直接进诊断。
```

**用户确认后**：
- 继续 → Read `goal-definition/SKILL.md` 并执行
- 跳过 → Read `readiness-diagnosis/SKILL.md` 并执行

---

## 中途信息不足的处理

- **不要阻塞流程**：记录已有信息，标注「低置信度，建议后续补充」
- 告知缺失对诊断的影响（高/中/低）
- 继续下一维度

例：「②数据准备度已记录，样本量缺失，影响数据维度评级精度（中等影响），可在诊断前补充。继续看③技术互联度，已完成 2/6。」
