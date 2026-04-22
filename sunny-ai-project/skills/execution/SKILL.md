---
name: execution
description: 工厂AI项目执行管理框架（阶段5）。提供Stage Gate门控评审标准、RACI矩阵、风险登记册、月报模板和Stage 1-4的工作分解，帮助PM跟踪项目执行进展。
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc Read Write
metadata:
  disable-model-invocation: "true"
---

# 执行管理框架（阶段 5）

**管理原则：** Agile-Stage-Gate × DMAIC × CRISP-DM 三轨合一。每阶段末 Gate 门控，硬标准全满足方可进下一阶段。

**参考：** Robert G. Cooper《Stage-Gate Process》· PMI PMBOK 7th · Six Sigma DMAIC · CRISP-DM 1.0 · McKinsey Rewired（2023）· WEF Lighthouse 规模化路径

---

## 🧭 内联问卷生成协议（本 skill 通用执行块）

本 skill 所有"问卷式输入点"**直接内联**调用 `deep_research`（深度研究）+ 飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）。

**A · deep_research（必做）** — query 的目标是**"向用户提问所用的访谈/评估方法论"**（SIPOC / GQM / 5W1H / A3 / CRISP-DM Business Understanding / Bernard Marr AI Use Case Canvas / NIST AI RMF Map / Hoshin X-Matrix / AIAG FMEA / Prosci ADKAR / Kotter 8-step / McKinsey Rewired Diagnostic 等），**不是**"问题的领域知识答案"。用户此刻还没给出完整问题，我们要的是"**怎么问才能把信息问全**"的访谈工具，不是"问题怎么解决"的答案。

query 构造：`[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议" + [intake goal 英文] + "intake framework / interview protocol / structured questionnaire / assessment checklist" + 列举 2-3 个真实方法论名`

权威来源：McKinsey Rewired / BCG AI at Scale / WEF Lighthouse / INCIT AIMRI / ISO 13053 / 55001 / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter / Basili GQM。

**B · 合成 3-5 条洞察**。
**C · 生成飞书** — 🟡/⚪；6:4；量化 ≥ 50%；每章节 ≤ 8 行；末尾「📌 填写说明」。
**D · 调用飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）**（失败降级为贴 markdown）。
**E · 回用户**：来源 + 链接 + 要点 3 条。
**F · 等贴回** → 解析 → 追问 🟡 → 低置信度继续。

---

## 入口询问

先问用户：
1. 当前 Stage（1/2/3/4）？
2. 需要什么支持（Gate 评审 / RACI / 风险登记 / 月报 / 工作分解）？

---

## 输入点 1：Stage 进度收集——执行内联问卷生成协议

用户告知所处 Stage 后，先生成该 Stage 的进度收集飞书文档。

差异参数：

```
research_query: "AI 项目 Stage 进度收集访谈框架 PMBOK monitoring phase interview Agile sprint review protocol DMAIC Control assessment questionnaire RACI checkpoint intake manufacturing AI project progress structured interview"
topic: "工厂 AI 项目 Stage {用户当前 Stage} 进度收集"
scenario: [项目名 + AI 类型 + 当前 Stage + 需要的支持类型]
industry: 舜宇光学 精密光电制造
doc_name: "[厂区代码]-[项目简称]-Stage{N}进度收集-[YYYYMMDD]"
purpose: "收集当前 Stage 执行现状，精准匹配 Gate 评审 / 风险预警 / 月报建议"
```

**问卷章节建议（按 Stage 差异化）：**
- Stage 里程碑完成度（🟡，对照四阶段行动清单逐项打 ✅/🟡/🔴）
- KPI 当前值 vs 基线 vs 目标（🟡）
- 已触发 / 新增风险（🟡 R01-R10 清单对照）
- 团队配置变化（🟡 PE/AIE/ITE 是否到位 / 离职）
- 阻碍清单（🟡 需 Sponsor 介入的 1-3 条）
- 下阶段 Gate 预判（⚪ 硬标准是否能满足）
- 外部依赖（⚪ 供应商 / 设备 / 客户配合）

话术：

> 先了解 Stage {N} 执行现状，再给针对性支持。
> 每字段 🟡 / ⚪——🟡 缺了影响建议精准度但不阻塞。

用户贴回后，针对性提供下方对应章节支持。

---

## 输入点 2：月报——执行内联问卷生成协议

用户选"月报"支持时：

差异参数：

```
research_query: "AI 项目月报状态收集访谈框架 PMBOK status report interview template SLC situation-learning-change questionnaire RAG status elicitation protocol manufacturing monthly review structured intake milestone KPI risk 阻碍访谈"
topic: "AI 调优项目月报收集"
scenario: [项目名 + 当前 Stage + 上月里程碑 + KPI 列表]
industry: 舜宇光学 精密光电制造
doc_name: "[厂区代码]-[项目简称]-月报-[YYYYMMDD]"
purpose: "帮 PM 快速完成月度汇报，生成结构化月报供管理层 Review"
```

**问卷章节建议**：本月完成（🟡 3-5 条）/ 下月计划（🟡 3-5 条）/ 里程碑状态（🟡 🟢/🟡/🔴）/ KPI 当前值（🟡）/ 风险更新（🟡 仅变化）/ 阻碍与决策请求（🟡 需 Sponsor 介入）/ 数据准备度维度变化（⚪）。

用户贴回后，生成管理层可用的月报输出（见下方月报模板）。

---

## 三轨框架

```
时间：   [ 90 天 ]──[   6 月   ]──[  12 月  ]──[  24 月  ]
Stage：   Stage 1      Stage 2       Stage 3     Stage 4
DMAIC：   Define      Measure+Analyze  Improve    Control
CRISP-DM：业务/数据理解 数据/建模/评估  部署/规模   监控/新周期
成熟度：  启动就绪     PoC 验证       工业化部署   制度化+扩展
```

---

## 核心角色与 RACI

### 角色

| 代码 | 名称 | 职责 | 各阶段投入 |
|---|---|---|---|
| Sponsor | 项目发起人 | 资源 / 预算 / 门控 | S1:20%→S4:10% |
| PM | 项目经理 | 进度 / 风险 / 汇报 | S1:60%→S4:30% |
| PE | 工艺工程师 | 工艺输入 / 参数 / DOE / 验证 | S1:60%→S3:80%→S4:40% |
| AIE | AI/数据工程师 | 特征 / 模型 / 部署 | S1:60%→S2:80%→S4:30% |
| ITE | IT/OT 工程师 | 管道 / 集成 / 安全 | S1:30%→S2:70%→S4:40% |
| QE | 质量工程师 | MSA / SPC / 质量数据 | S1:50%→S2:60%→S4:20% |
| EE | 设备工程师 | 设备接口 / 参数写入 | S1:20%→S2:50%→S3:40% |
| OPS | 生产主管 | 操作员培训 / 现场 | S1:20%→S3:60%→S4:80% |
| CHG | 变革管理 | 阻力化解 / 培训 | S2:30%→S3:50%→S4:60% |

> 必须角色：Sponsor / PM / PE / AIE。QE/EE/ITE 视场景。CHG 在 Stage 3 规模化时关键。

### RACI 矩阵

> R = 执行 / A = 决策 / C = 咨询 / I = 知会

| 核心活动 | Sponsor | PM | PE | AIE | ITE | QE | EE | OPS | CHG |
|---|---|---|---|---|---|---|---|---|---|
| 项目章程签批 | **A** | R | C | I | I | I | I | I | I |
| 六维准备度评估 | I | A | R | C | C | R | C | C | I |
| 数据地图与质量 | I | I | C | R | **A** | C | I | I | I |
| MSA/GR&R | I | I | C | I | I | **R/A** | C | I | I |
| PoC 模型开发 | I | I | C | **R/A** | C | C | I | I | I |
| 数据管道 | I | I | I | C | **R/A** | I | C | I | I |
| 操作员培训 | I | A | C | I | I | I | I | R | **R** |
| Stage Gate 评审 | **A** | R | C | C | C | C | I | I | I |
| 月报汇报 | I | **R/A** | C | C | I | C | I | I | I |

---

## Stage Gate 门控标准

### Gate 1（Stage 1 末，约 90 天）

硬标准：
- [ ] G1.1 数据可行：X-Y 关联率 ≥ 60%，或 Stage 2 内可达 80%+
- [ ] G1.2 测量可信：GR&R < 30%，或有改善计划
- [ ] G1.3 设备接口：采集方案确认
- [ ] G1.4 PoC 方案：Sponsor 审批通过
- [ ] G1.5 团队就绪：PE + AIE + ITE 到位

有目标校准 → 额外：
- [ ] 校准后阶段性目标已认可

### Gate 2（Stage 2 末，约 6 月）

- [ ] G2.1 PoC 精度：验证集 ≥ 目标 70%
- [ ] G2.2 管道稳定：自动采集 ≥ 2 周无中断；延迟达标
- [ ] G2.3 GR&R 达标：< 30%（若 G1.2 是"有计划"，此时必须达）
- [ ] G2.4 PoC 实效：采纳 ≥ 50%；可量化 KPI 改善
- [ ] G2.5 规模化计划：Stage 3 计划获批（含培训 / IT / 预算）

### Gate 3（Stage 3 末，约 12 月）

- [ ] G3.1 规模化覆盖 ≥ 70%
- [ ] G3.2 采纳连续 4 周 ≥ 70%（日志支撑）
- [ ] G3.3 模型稳定：PSI < 0.25；精度无超阈值下降
- [ ] G3.4 无重大损失：未因 AI 误判致重大质量事故
- [ ] G3.5 ROI：实际改善 ≥ 预期 60%；全年 ROI 正

### Gate 4（Stage 4 末，约 24 月）— 项目关闭

- [ ] G4.1 ROI 达成率 ≥ 80%
- [ ] G4.2 自主运营：无 PM 介入稳定运行 ≥ 3 月
- [ ] G4.3 方法论复制：第 2 工序已立项，用本项目框架
- [ ] G4.4 知识资产：Playbook 发布并验证；知识库 ≥ 20 案例

---

## 风险登记册（参考）

| # | 风险 | 可能性 | 影响 | 等级 | 阶段 | 缓解 | 责任 | 状态 |
|---|---|---|---|---|---|---|---|---|
| R01 | X-Y 关联率 < 50% | 中 | 高 | 高 | S1 | 早期数据地图；DoE 备用 | ITE/AIE | 开放 |
| R02 | GR&R > 30% | 中 | 高 | 高 | S1 | MSA 第 1 月完成；差即改 | QE | 开放 |
| R03 | PoC 过拟合 | 中 | 高 | 高 | S2 | 严格划分；正则；跨批次 CV | AIE | 开放 |
| R04 | IT/OT 隔离数据无法传 | 高 | 中 | 高 | S1 | 提前 3 月 IT 审批；离线批量 | ITE | 开放 |
| R05 | 数据漂移精度下降 | 中 | 中 | 中 | S3+ | PSI 监控；再训练 SOP | AIE | 开放 |
| R06 | 缺陷样本稀少 | 中 | 高 | 高 | S2 | GAN 增强；迁移学习；PatchCore | AIE/PE | 开放 |
| R07 | 操作员抵制 | 中 | 高 | 高 | S3 | 班长早期参与；建议模式；透明化 | CHG/OPS | 开放 |
| R08 | 关键角色离职 | 低 | 高 | 中 | 全程 | 文档化；备份；交接 SOP | PM | 开放 |
| R09 | 供应商配合低 | 中 | 中 | 中 | S1-2 | 合同条款；提前 6 月沟通；备用 | EE/PM | 开放 |
| R10 | 范围蔓延 | 高 | 中 | 高 | S2+ | 严格变更管理；Sponsor 审批 | PM | 开放 |

---

## 月报模板（PM 每月填写，Review 前 2 天发出）

```
【AI 调优项目月报】
月份：______  PM：______  当前 Stage：__

▶ 整体：🟢按计划 / 🟡轻微偏差 / 🔴重大风险

▶ 本月完成
  - [工作包编号] 内容

▶ 下月计划
  - [工作包编号] 内容

▶ 里程碑
  - [M#] 名称：🟢 / 🟡 延迟 N 天 / 🔴 偏差（原因 ___）

▶ KPI 当前值
  - [指标]：基线 ___ / 当前 ___ / 目标 ___ / 状态 🟢/🟡/🔴

▶ 风险更新（仅变化）
  - [R#] 状态变化说明

▶ 阻碍与决策请求（需 Sponsor）
  - [问题]  期望决策时间：______

▶ 数据准备度更新（有实质变化时）
  - [维度] X 级 → Y 级（原因 ___）
```

---

## 完成后衔接

> 项目执行过程中，每季度可启动 `/sunny-ai-project:iteration`，提交新增数据和进展，获取更新后的评级与行动调整建议。
