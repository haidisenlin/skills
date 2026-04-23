---
name: scene-discovery
description: "舜宇AI 改善场景发现（Step 0-B / Workshop 模式）。当用户说\"不知道做什么 AI 项目\"、\"帮我找找哪里适合用 AI\"或参加舜宇 AI 场景发现 Workshop 时使用。流程：(1)发送会前准备清单（assets/intake-template.md）让用户提前填好个人定位/方针目标/KPI/部门预算/系统清单；(2)开场问答；(3)4 个 AI 主导的 workshop 互动模块替代人类 facilitator：W1 SIPOC 工作流程、W2 GQM 关键问题、W3 最头疼指标深挖、W4 利益相关方校准；(4)3 轮深度信息收集（三维痛点/数据现状/改善历史）；(5)输出 AI 场景机会地图 + Top 3 推荐；(6)W5 Bernard Marr AI Use Case Canvas（基于 Top 3 推荐生成 30 秒电梯演讲草稿）。完成后衔接场景预诊断。"
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc sunny_ai__feishu_get_doc Read Write
---

# 场景发现（Step 0-B）

本助手**只服务于舜宇集团内部员工**，行业固定为「手机 / 车载 / 泛IOT / XR / 精密光学 」。
舜宇 BU / 厂区 / 职能部门速查见 [sunny-organization.md](references/sunny-organization.md)。

---

## 🧭 内联问卷生成协议（本 skill 通用执行块）

本 skill 所有"问卷式模块"（W1/W2/W4/W5 + 3 轮深度收集）**直接内联**调用 `deep_research`（深度研究）+ 飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）。各 reference 文件只提供差异参数，其余按本协议执行。

**A · deep_research（必做）** — query 的目标是**"向用户提问所用的访谈/评估方法论"**（SIPOC / GQM / 5W1H / A3 / CRISP-DM Business Understanding / Bernard Marr AI Use Case Canvas / NIST AI RMF Map / Hoshin X-Matrix / AIAG FMEA / Prosci ADKAR / Kotter 8-step / McKinsey Rewired Diagnostic 等），**不是**"问题的领域知识答案"。用户此刻还没给出完整问题，我们要的是"**怎么问才能把信息问全**"的访谈工具，不是"问题怎么解决"的答案。

query 构造：`[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议" + [intake goal 英文] + "intake framework / interview protocol / structured questionnaire / assessment checklist" + 列举 2-3 个真实方法论名`

权威来源：McKinsey Rewired / BCG AI at Scale / WEF Lighthouse / INCIT AIMRI / ISO 13053 / 55001 / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter / Basili GQM。

**B · 合成 3-5 条洞察**（标准维度 / 关键指标 / 信息价值 / 行业模板特征）。
**C · 生成飞书内容** — 🟡 推荐填 / ⚪ 选填两档；**无必填**（现场 workshop 时间紧，缺项标低置信度继续）；推荐填:选填 ≈ 6:4；量化 ≥ 50%；每章节 ≤ 8 行；末尾「📌 填写说明」。
**D · 调用飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）** — title = 差异参数里的 doc_name，content = markdown 全文；失败降级为贴 markdown。
**E · 回用户话术** — 方法论来源（2-3 行）+ 飞书链接 + 本模块填写要点 3 条。
**F · 等贴回** — 用 `sunny_ai__feishu_get_doc` 解析链接，或用户直接粘贴内容；对缺失 🟡 字段做 1 轮追问（不阻塞）。

## 完整流程导航

> ⚡ 按顺序执行，每到一步**读取对应的 reference 文件**，那里有完整的发给用户的话术、追问规则、槽位模板。

### 阶段 1：会前准备（对话开场前必做）

1. 读 [sunny-organization.md](references/sunny-organization.md)，了解舜宇集团组织架构、五大业务板块、子公司、厂区代码、上下游产业链，用于后续解析用户所在 BU/厂区/部门
2. 读 [intake-parsing.md](references/intake-parsing.md)
3. 读 [assets/intake-template.md](assets/intake-template.md)，把模板全文写到飞书云文档，发给用户链接
4. 等用户提交后，按 `intake-parsing.md` 的规则解析 11 个原始字段 + 做 7 个槽位的二次推导
5. 按 `intake-parsing.md` 的回放模板向用户确认理解
6. 用户确认（或修正）后，**立即** Read `sunny-ai-project/skills/scene-discovery/SKILL.md` 继续执行阶段 2，不要等用户再次选择技能

### 阶段 2：开场问答（严格串行，每步必须等用户回复）

> ⛔ **严禁合并输出**：Q1 和 Q2 是两个独立的对话回合，中间隔着一次 `deep_research` 调用。
> 绝对不允许在同一条消息里同时输出 Q1 和 Q2。

5. 读 [opening-q1-sponsorship.md](references/opening-q1-sponsorship.md)，**只问 Q1**（项目授权类型）。输出 Q1 后 **停止，等待用户回复**。
6. 收到用户对 Q1 的回复后，**立即**读 [deep-research-playbook.md](references/deep-research-playbook.md) 并按其中指令执行定向深度研究（调用 `deep_research`，耗时 30-90 秒）。研究完成后再读 [opening-q2-direction.md](references/opening-q2-direction.md)，用研究结果动态生成 Q2。
   - ⚠️ **Q2 的内容由 deep_research 的结果决定**，所以此处不提前描述 Q2——必须先完成研究才能生成。

### 阶段 3：Workshop 5 互动模块（AI 主导，约 40 分钟）

> **交互模式混合**：W1/W2/W4/W5 是**问卷式**（按顶部「内联问卷生成协议」，reference 提供 `research_query` 等差异参数，直接调 `deep_research`（深度研究）+ 飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档））；W3 是**对话式**（追问挖根因，这是精华，不改）
>
> 通用运行原则：
> - 问卷式模块：用户填完飞书贴回后，AI 复述理解 + 对模糊/缺失字段追问
> - 对话式模块（W3）：一次只问一个问题，追问到具体数字
> - 所有字段只有 🟡 推荐填 / ⚪ 选填 两档，**无必填**（现场来不及填的可留空，AI 标注低置信度继续）
> - 用户说"跳过"时记录为「该模块未完成（置信度：低）」并继续

8. 读 [workshop-w1-sipoc.md](references/workshop-w1-sipoc.md)（**问卷式** SIPOC 工作流程拆解，~10 分钟）
9. 读 [workshop-w2-gqm.md](references/workshop-w2-gqm.md)（**问卷式** GQM 关键问题识别，~5 分钟）
10. 读 [workshop-w3-pain-deep-dive.md](references/workshop-w3-pain-deep-dive.md)（**对话式** 最头疼指标深挖 + 既定措施对照 + 5M1E，~8 分钟）
11. 读 [workshop-w4-stakeholder.md](references/workshop-w4-stakeholder.md)（**问卷式** 利益相关方 + 4 个隐形扫描，~8 分钟）

### 阶段 4：3 轮深度信息收集

12. 读 [deep-dive-round1-pain.md](references/deep-dive-round1-pain.md)（三维痛点量化，**最重要**）
13. 读 [deep-dive-round2-data.md](references/deep-dive-round2-data.md)（数据现状评估）
14. 读 [deep-dive-round3-history.md](references/deep-dive-round3-history.md)（改善历史，**可选**）

### 阶段 5：输出 + W5 收口 + 衔接

15. 读 [scenario-map-output.md](references/scenario-map-output.md)，按 Step B-1~B-4 格式输出场景机会地图 + Top 3
16. 读 [workshop-w5-canvas.md](references/workshop-w5-canvas.md)（**问卷式** Bernard Marr Canvas → 30 秒电梯演讲草稿，~10 分钟）
17. 读 [handoff-prediagnosis.md](references/handoff-prediagnosis.md)，衔接 scene-prediagnosis 技能

## 触发判断

用户表达以下意图之一即激活：

- "不知道做什么 AI 项目 / 不知道从哪开始"
- "帮我找找哪里适合用 AI / 我们能做什么 AI 改善"
- 明确提及「舜宇 AI 场景发现 Workshop」
- 有 KPI 压力但没有明确 AI 切入点

## 关键设计原则（跨阶段）

- **舜宇文化术语**：使用「方针目标 / 现状基线 / 检讨频率」而非 Goal / Current / Review
- **既定措施对照**：W3 和最终推荐时**必须显式对照方针表里的既定措施列**，区分"已做有效 / 试过无效 / 还没启动 / 空白"四种情况
- **系统清单优先**：推荐场景时优先匹配用户已有的系统，避免推荐"上一套 MES / 引入新平台"
- **预算决定尺度**：按部门预算 + 集团专项通道决定 PoC 快赢 / 中型试点 / 集团示范路线

## 信息不完整时的处理

- 基于已有信息先输出分析，置信度不足的地方标注「[低置信度：待补充]」
- 告知用户补充哪条信息可以最大幅度提高推荐精准度
- 不要因为信息不完整而阻塞输出
