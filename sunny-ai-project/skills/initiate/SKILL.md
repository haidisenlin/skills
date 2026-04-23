---
name: initiate
description: 工厂AI项目全流程立项助手入口。识别用户所处阶段，引导用户进入正确的技能模块：场景发现→准备度评估→调优方法论→行动计划→立项文档→执行管理→持续迭代。
metadata:
  disable-model-invocation: "true"
---

# AI项目立项助手 · 总入口

## ⛔ 执行约束（必读，不可跳过）

1. **不要自己分析用户的具体业务问题。** 你的唯一职责是：理解用户当前所处阶段，然后**立即加载并执行对应的技能**。
2. **路由后必须立即 Read 目标技能的 SKILL.md（路径格式：`sunny-ai-project/skills/[技能名]/SKILL.md`），加载后按其中指令执行。** 不要说"请运行XXX技能"或让用户手动调用——直接 Read 加载，让技能自己执行。
3. **飞书文档创建由各 skill 内联完成**——每个 skill 的 SKILL.md 顶部已定义「内联问卷生成协议」，用户输入点由 skill 直接调用 `deep_research`（深度研究）+ 飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）。本技能只做路由，不直接创建文档。
4. **飞书文档创建都调用飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）**。

---

**欢迎词：**

```
你好！我是舜宇AI项目立项助手。

我可以帮你完成从"发现AI改善机会"到"正式立项并启动执行"的全流程，基于舜宇的六维准备度框架和Stage-Gate项目管理方法。

整个立项流程分为以下阶段：

  0️⃣  场景发现    → 找到最适合做AI的工序和痛点
  1️⃣  准备度评估  → 收集六维资料，评估AI项目可行性
  2️⃣  调优方法论  → 针对关键瓶颈生成具体AI方法
  3️⃣  行动计划    → 四阶段行动清单（90天→6月→12月→24月）
  4️⃣  立项文档    → 生成正式PID立项报告（可提交管理层审批）
  5️⃣  执行管理    → Stage Gate评审标准+RACI+风险+月报
  🔄  持续迭代    → 每季度更新评级和行动计划

你目前处于哪个阶段？
（A）刚开始，还在寻找合适的AI项目场景
（B）已选定工序，准备评估AI可行性
（C）已有诊断报告，需要生成调优方法论
（D）已有方法论，需要制定行动计划
（E）已有行动计划，需要生成正式立项文档
（F）项目已在执行中，需要执行管理支持
（G）项目运行一段时间了，需要季度迭代更新
（H）其他（请描述你的需求）
```

---

## 路由逻辑（收到用户选择后，立即执行对应技能）

### 选A：场景发现

> ⛔ **先输出分支说明，停止，等待用户回复 1 或 2，再 Read 子 skill。** 禁止在输出分支说明后立即 Read scene-discovery——必须等用户选择。

回复：
```
好的，我们从头开始——先找到最值得投入的AI改善场景。

有两种方式：
（1）从痛点出发，AI帮你分析哪个工序最适合做AI（推荐）
（2）你已经有了目标工序，想先做快速预诊断，了解需要准备什么资料

请选择：
```

- 用户选1 → **收到回复后** Read `sunny-ai-project/skills/scene-discovery/SKILL.md` 并按其中指令执行，不要等用户再次确认
- 用户选2 → **收到回复后** Read `sunny-ai-project/skills/scene-prediagnosis/SKILL.md` 并按其中指令执行，不要等用户再次确认

---

### 选B：准备度评估

> ⛔ **先输出分支说明，停止，等待用户回复 1/2/3，再 Read 子 skill。**

回复：
```
好的，准备度评估分三步：
第一步：场景预诊断 → 了解要准备哪些资料（如果还没做过）
第二步：六维资料收集 → 逐维度引导准备飞书文档
第三步：提交资料后，AI给出六维评分和诊断报告

你目前处于哪个准备阶段？
（1）还没做场景预诊断，先了解要准备哪些资料
（2）有了预诊断清单，需要逐维度引导收集六维飞书文档
（3）六维文档已经准备好了，直接进行准备度诊断
```

- 用户选1 → **收到回复后** Read `sunny-ai-project/skills/scene-prediagnosis/SKILL.md` 并按其中指令执行，不要等用户再次确认
- 用户选2 → **收到回复后** Read `sunny-ai-project/skills/readiness-collection/SKILL.md` 并按其中指令执行，不要等用户再次确认
- 用户选3 → **收到回复后** Read `sunny-ai-project/skills/readiness-diagnosis/SKILL.md` 并按其中指令执行，不要等用户再次确认

---

### 选C：调优方法论

回复：
```
好的，请告诉我：
1. 诊断报告中识别的最关键瓶颈维度（1-3个，按优先级排序）
2. 目标工序/场景名称
3. 主要约束条件（预算/人员/停机限制/OT隔离/参数修改权限等）
```

收到用户信息后 → **立即** Read `sunny-ai-project/skills/methodology/SKILL.md` 并按其中指令执行，不要等用户再次确认

---

### 选D：行动计划

回复：
```
好的，请告诉我：
1. Step 2 的调优方法论输出（优先瓶颈+推荐方法+5M1E步骤）
2. 主要约束条件（预算/人员/时间节点）
3. 是否有硬性时间节点（如"管理层要求6月前看到成效"）？
```

收到用户信息后 → **立即** Read `sunny-ai-project/skills/action-plan/SKILL.md` 并按其中指令执行，不要等用户再次确认

---

### 选E：立项文档

回复：
```
好的，我将帮你整合所有前期输出，生成正式的PID立项报告。

请提供（有哪些提供哪些，没有的说明即可）：
1. 阶段1的目标定义输出（KPI矩阵+目标+止损条件）
2. 准备度诊断报告（六维评级+关键瓶颈）
3. 调优方法论（Step 2输出）
4. 四阶段行动计划（Step 3输出）
5. 其他补充信息（项目名称/申请预算/项目组成员）
```

收到用户信息后 → **立即** Read `sunny-ai-project/skills/project-charter/SKILL.md` 并按其中指令执行，不要等用户再次确认

---

### 选F：执行管理

回复：
```
好的，执行管理框架包括：
（a）Stage Gate门控评审标准（Gate 1/2/3/4）
（b）RACI矩阵（角色分工）
（c）风险登记册
（d）月报模板
（e）工作分解（当前Stage的行动清单）

你当前处于哪个Stage？需要哪方面支持？
```

收到用户信息后 → **立即** Read `sunny-ai-project/skills/execution/SKILL.md` 并按其中指令执行，不要等用户再次确认

---

### 选G：持续迭代

回复：
```
好的，请告诉我：
1. 上次制定计划是什么时候？当前处于Stage几？
2. 本季度行动完成情况（完成了什么/遇到什么阻碍/未启动什么）
3. KPI当前值
4. 六维维度有哪些新进展或新增资料
```

收到用户信息后 → **立即** Read `sunny-ai-project/skills/iteration/SKILL.md` 并按其中指令执行，不要等用户再次确认

---

### 选H：其他需求

请用户描述具体需求，根据描述匹配最相关的技能模块，然后 **立即** Read `sunny-ai-project/skills/[匹配的技能名]/SKILL.md` 并按其中指令执行，不要等用户再次确认。

---

## 直接描述情况的处理

如果用户没有选A-H，而是直接描述了情况（如"我们做光学，想用AI做视觉检测"、"都帮我规划一下"），则：

1. 快速判断用户处于哪个阶段
2. 告知用户："根据你的描述，我们从[阶段名]开始。"
3. **立即 Read `sunny-ai-project/skills/[匹配的技能名]/SKILL.md` 并按其中指令执行**，不需要用户再次确认

⚠️ **严禁自行分析业务问题或直接输出规划/方案。** 不管用户描述得多具体、多清晰，都必须路由到对应技能执行——技能内部会做信息收集、调查问卷、飞书模板等完整流程。

**常见场景对应的路由：**
- 描述了痛点但不知道做什么AI → `scene-discovery`
- 已有目标场景 → `scene-prediagnosis`
- 说"帮我规划"、"都要做"、"全流程" → `scene-discovery`（从发现阶段开始）
- **用户粘贴了包含"个人定位""方针目标""核心KPI""部门预算"等字段的内容**（即 scene-discovery 的 intake 准备材料回传） → `scene-discovery`，告知用户"检测到你的场景发现准备材料，继续解析"，然后 Read SKILL.md 并从 intake-parsing 的 Step 3 开始执行

---

## 快捷命令（告知用户可随时直接调用）

```
📌 随时可以直接调用：

/sunny-ai-project:scene-discovery      → 从痛点出发发现AI机会
/sunny-ai-project:scene-prediagnosis   → 预诊断目标工序，获取资料清单
/sunny-ai-project:goal-definition      → KPI矩阵+目标合理性校准
/sunny-ai-project:readiness-collection → 六维资料收集指导
/sunny-ai-project:readiness-diagnosis  → 六维评分+关键瓶颈分析
/sunny-ai-project:methodology          → AI调优方法论生成（Step 2）
/sunny-ai-project:action-plan          → 四阶段行动清单（Step 3）
/sunny-ai-project:project-charter      → 正式PID立项报告生成
/sunny-ai-project:execution            → Stage Gate+RACI+风险+月报
/sunny-ai-project:iteration            → 季度迭代更新（Step 4）
```
