---
name: readiness-diagnosis
description: 工厂AI项目六维准备度AI诊断（Step 1 + 1.5）。当用户已完成六维资料收集（或准备好飞书文档链接/资料描述），需要获得1-5级准备度评分、关键瓶颈分析和AI启动建议时使用。目标设置激进时自动触发Step 1.5目标校准。完成后衔接方法论生成。
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc Read Write
---

# 准备度诊断（Step 1 + Step 1.5）

**方法论来源：** McKinsey Rewired AI 准备度 · BCG AI at Scale · INCIT AIMRI 成熟度 · WEF Lighthouse 瓶颈识别（5M1E）

**前提：** 已完成阶段 1（目标定义）与阶段 2（六维资料收集）。

---

## 🧭 内联问卷生成协议（本 skill 通用执行块）

本 skill 所有"问卷式输入点"**直接内联**调用 `deep_research`（深度研究）+ 飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）。

**A · deep_research（必做）** — query 的目标是**"向用户提问所用的访谈/评估方法论"**（SIPOC / GQM / 5W1H / A3 / CRISP-DM Business Understanding / Bernard Marr AI Use Case Canvas / NIST AI RMF Map / Hoshin X-Matrix / AIAG FMEA / Prosci ADKAR / Kotter 8-step / McKinsey Rewired Diagnostic 等），**不是**"问题的领域知识答案"。用户此刻还没给出完整问题，我们要的是"**怎么问才能把信息问全**"的访谈工具，不是"问题怎么解决"的答案。

query 构造：`[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议" + [intake goal 英文] + "intake framework / interview protocol / structured questionnaire / assessment checklist" + 列举 2-3 个真实方法论名`

权威来源：McKinsey Rewired / BCG AI at Scale / WEF Lighthouse / INCIT AIMRI / ISO 13053 / 55001 / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter / Basili GQM。

**B · 合成 3-5 条洞察**（维度/指标/信息价值/模板特征）。
**C · 生成飞书内容** — 🟡 推荐填 / ⚪ 选填两档；6:4 比例；量化 ≥ 50%；每章节 ≤ 8 行；末尾加「📌 填写说明」。
**D · 调用飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）**（title = doc_name，content = markdown）；失败降级为贴 markdown。
**E · 回用户**：方法论来源（2-3 行）+ 飞书链接 + 填写要点 3 条。
**F · 等贴回**：解析 → 追问缺失 🟡 → 低置信度继续。

---

## 第一步：诊断输入整合

在正式输出诊断前，AI 先自动整合前序 skill 的输出，再让用户补充增量信息。

### 1a. AI 自动整合（不需要用户操作）

从当前对话上下文中提取以下已有信息，预填到诊断输入文档：

- **阶段 1 KPI 矩阵**：从 goal-definition 输出中提取主 KPI/基线/目标/雄心度/OKR 信心
- **六维飞书文档链接**：从 scene-prediagnosis 六维收集中提取已创建的 6 份飞书链接
- **六维关键数据摘要**：从六维收集的用户填写回传中提取每维度 2-3 个关键数据点

AI 将上述信息自动填入文档，并在每项后标注「🤖 已自动填充 — 如有错误请修正」。

### 1b. 用户补充增量信息——执行内联问卷生成协议

只让用户填写 AI 无法自动获取的新增信息。

差异参数：

```
research_query: "AI 准备度诊断输入汇总访谈框架 六维综合档案整合问卷 McKinsey Rewired diagnostic protocol INCIT AIMRI assessment interview BCG AI at Scale diagnostic questionnaire manufacturing AI readiness intake aggregation interview"
topic: "工厂 AI 项目六维准备度诊断 · 增量补充"
scenario: [用户已完成的目标定义 + 六维资料收集飞书链接]
industry: [用户行业]
doc_name: "[厂区代码]-[项目简称]-准备度诊断输入-[YYYYMMDD]"
purpose: "在 AI 自动整合的基础上，补充完整度自评、关切维度和近期变化"
```

**问卷章节建议**（仅用户增量部分）：
- 各维信息完整度自评（🟡 高/中/低 × 6 维）
- 最担心的 2-3 个维度及原因（🟡）
- 自动填充内容修正（⚪；如有数据变化或错误）
- 补充上下文：近 3 月异常事件（⚪）/ 其他改善项目关联（⚪）

话术：

> 我已从前序步骤自动整合了 KPI 矩阵和六维资料。请检查下方预填内容是否准确，
> 然后补充"完整度自评"和"最担心的维度"即可。
> 完成后粘贴链接，我会逐一审阅六维文档，生成完整诊断报告。

---

## 诊断报告输出格式

### 【六维准备度评级】

```
① 战略价值度：X 级（置信度：高/中/低）
   评级依据：[基于用户描述，引用关键数据]
   信息缺口影响：[若有缺口说明对评级的影响]

② 数据准备度：X 级（置信度：...）
   评级依据：[D1-D6 各子维度关键依据]
   ...

③ 技术互联度 / ④ 工艺可控度 / ⑤ 组织能力度 / ⑥ 管理变革度：同上格式
```

评级标准见 [references/scoring-criteria.md](references/scoring-criteria.md)。

<details><summary>📐 评级速查（完整标准见上方链接）</summary>

| 维度 | 1级 | 3级（最低启动线） | 5级 |
|---|---|---|---|
| ①战略价值度 | 无量化/无Owner/无ROI | 量化清楚+有实权Owner+ROI合理 | 纳入OKR+系统追踪+可对外发布 |
| ②数据准备度 | D1-D6多数1-2级 | D1-D6均≥3级且无<2级 → **可启动PoC** | D1-D6全≥4级，D3/D6达5级 |
| ③技术互联度 | 无数字接口/纯手抄 | OPC-UA/MQTT标准接口+MES有数据+延迟<1h | 全程数字主线+实时数据湖 |
| ④工艺可控度 | 参数未识别/无GR&R | GR&R 10-30%+参数调整窗口 | 实时SPC+AI闭环调参 |
| ⑤组织能力度 | 无AI能力/知识口传 | 有AIE（或外包）+工艺文档+IT配合 | 内部AI CoE+完全融合 |
| ⑥管理变革度 | 明确抵制/审批>1月 | 关键班长支持+审批<1周+有成功历史 | AI推荐=标准操作+精益文化 |

综合3级 = **最低启动线**（可开始小范围PoC）；综合<2.5且改善>15% → 触发目标校准。

数据准备度关键子维度：D1质量(GR&R) / D2追溯(X-Y关联率) / D3数据量 / D4标注 / D5漂移 / D6治理。

</details>

### 【诊断结论】

```
综合评分：X.X / 5
是否达到启动门槛：是/否（原因）

启动建议：
□ 全量启动
□ 单台设备 PoC
□ 先完成前置工作（清单，预计 N 月）
```

### 【最关键瓶颈（5M1E 根因分解）】

对最关键的 2-3 个瓶颈维度：

```
瓶颈① [维度]（当前 X 级 → 需 Y 级支撑目标）
对目标的影响：[不解决，目标实现概率下降 X%]
5M1E：
  人：[如"AIE 缺失，工艺专家知识未文档化"]
  机：[如"设备无数字接口"]
  料：[如"原料批次未关联生产数据"]
  法：[如"GR&R 未做"]
  环：[如"IT/OT 隔离"]

瓶颈② ...
```

### 【资料补充建议】

```
P1：补充 [信息/文件]
    理由：将使 [维度] 置信度低→高，可能从 X 级提升至 Y 级
    收集难度：低/中/高
P2：...
P3：...
```

### 【目标-现实校准评估】⚠️ 条件触发

**触发条件（任一）：**
- 雄心度 🟠 激进 / 🔴 变革性
- OKR 信心 ≤ 4
- 综合 < 2.5 且改善幅度 > 15%

未触发 → 标注"✅ 目标与准备度匹配"。

触发时输出：

```
目标雄心度：🟢/🟡/🟠/🔴
当前综合准备度：X.X / 5
支撑该目标所需最低（WEF Lighthouse 同类）：≥ X 级
关键缺口：[差距最大的 1-2 个维度]

可达性判定：
□ ✅ 匹配
□ ⚠️ 偏激进（仅能实现 X%，建议三选一）
□ ❌ 严重错位（必须修正）

校准选项（⚠️/❌ 必须三选一）：
A · 调整目标值：
  → 匹配当前准备度：___ / [Y] 月
  → 匹配 Stage 1 完成后：___ / [Z] 月
B · 扩展时间线：
  → 原目标延至 ___ / 前置 [N] 月
C · 分阶段设目标（推荐优先）：
  → 90 天：___（低风险可见成效）
  → [6-12] 月：___（规模化）
  → [12-24] 月：___（完整）
```

---

## Step 1.5：目标校准深度交互——执行内联问卷生成协议

**方法论：** McKinsey "Ambition-Reality Calibration" · OKR 渐进拆解（Google re:Work）· Stage-Gate（Robert Cooper）

**触发：** Step 1 判定为 ⚠️ 或 ❌。

差异参数：

```
research_query: "AI 项目目标校准约束与资源访谈框架 McKinsey ambition reality calibration interview OKR recalibration questionnaire Stage-Gate decision interview protocol 不可变约束与可调资源结构化访谈 manufacturing AI goal recalibration intake"
topic: "工厂 AI 项目目标校准 · 约束补充与方案选择"
scenario: [用户的诊断报告 + 选择的校准选项 A/B/C]
industry: [用户行业]
doc_name: "[厂区代码]-[项目简称]-目标校准-[YYYYMMDD]"
purpose: "收集不可改变的约束 + 可调整资源，产出校准后的目标卡片"
```

**问卷章节建议**：校准选项选择（🟡 A/B/C）/ 不可改变约束清单（🟡，如"管理层已对外承诺"）/ 可调整资源清单（🟡，如"可加 AIE 投入"）/ 阶段里程碑时间窗偏好（🟡）/ 失败容忍度（⚪）。

**输出校准目标卡片：**

```
短期里程碑（90 天）：___（可验证 / 低风险）
中期目标（[X] 月）：___（准备度建设完成后）
战略目标（[Y] 月）：___（原始或校准版）

置信度：低/中/高，理由：...

目标可达性最敏感的维度（Stage 1 必须优先提升）：
1. [维度] — 当前 X 级 → 需 Y 级，预计 N 周
2. ...

如坚持原目标：成功率 [X%]，主要风险：[1-2 个]

WEF Lighthouse / 同行业案例支撑：
[可信数据引用]
```

---

## 完成后衔接

```
✅ 准备度诊断完成！下一步【调优方法论生成】——基于瓶颈推荐 AI 算法、
   最低数据需求和 5M1E 步骤。

继续？（"是"/"继续"即可）
```

用户确认 → **立即** Read `sunny-ai-project/skills/methodology/SKILL.md` 并按其中指令执行，传入诊断输出（六维评级 / 优先瓶颈 / 目标校准）作为上下文，不要等用户再次确认。

---

## 信息不足降级

- 某维度严重不足 → 给"低置信度"评分 + 标注"评级范围：X-Y 级"
- 补充建议将该维度列为 P1
- 综合结论标注"中/低置信度"
- 不要因信息不足拒绝输出
