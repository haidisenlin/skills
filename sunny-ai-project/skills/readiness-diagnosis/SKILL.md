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

本 skill 所有"问卷式输入点"**直接内联**调用 `deep_research` + `sunny_ai__feishu_create_doc`，**不经 research-template**。

**A · deep_research（必做）** — 构造 query：`[topic 中文] + [scenario] + "评估框架 最佳实践 制造业 AI" + 英文`。权威来源：McKinsey / BCG / WEF / INCIT / ISO / AIAG-VDA。
**B · 合成 3-5 条洞察**（维度/指标/信息价值/模板特征）。
**C · 生成飞书内容** — 🟡 推荐填 / ⚪ 选填两档；6:4 比例；量化 ≥ 50%；每章节 ≤ 8 行；末尾加「📌 填写说明」。
**D · 调 `sunny_ai__feishu_create_doc`**（title = doc_name，content = markdown）；失败降级为贴 markdown。
**E · 回用户**：方法论来源（2-3 行）+ 飞书链接 + 填写要点 3 条。
**F · 等贴回**：解析 → 追问缺失 🟡 → 低置信度继续。

---

## 第一步：诊断输入整合——执行内联问卷生成协议

在正式输出诊断前，先让用户整合一份"诊断输入清单"飞书文档。

差异参数：

```
research_query: "industrial AI readiness diagnosis input checklist six dimensions strategic data technical process organizational change maturity assessment McKinsey BCG WEF Lighthouse INCIT AIMRI"
topic: "工厂 AI 项目六维准备度诊断 · 输入清单"
scenario: [用户已完成的目标定义 + 六维资料收集飞书链接]
industry: [用户行业]
doc_name: "[厂区代码]-[项目简称]-准备度诊断输入-[YYYYMMDD]"
purpose: "整合目标定义、六维资料链接和基线数据，为 AI 诊断提供完整上下文"
```

**问卷章节建议**：
- 阶段 1 KPI 矩阵摘要（🟡；主 KPI/基线/目标/雄心度/OKR 信心）
- 六维飞书文档链接（🟡 6 行全填；缺哪维标"待补"）
- 各维信息完整度自评（🟡 高/中/低）
- 最担心的 2-3 个维度（🟡）
- 补充上下文：近 3 月异常事件（⚪）/ 其他改善项目关联（⚪）

话术：

> 诊断前先整合输入。把阶段 1 的 KPI 摘要 + 六维飞书链接 + 基线数据填进这份文档。
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
research_query: "AI project goal calibration ambition reality stage gate phased milestone manufacturing AI target recalibration McKinsey OKR confidence 目标校准 阶段里程碑"
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

用户确认 → Read `methodology/SKILL.md` 执行，传入诊断输出（六维评级 / 优先瓶颈 / 目标校准）作为上下文。

---

## 信息不足降级

- 某维度严重不足 → 给"低置信度"评分 + 标注"评级范围：X-Y 级"
- 补充建议将该维度列为 P1
- 综合结论标注"中/低置信度"
- 不要因信息不足拒绝输出
