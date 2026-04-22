# Skills 目录说明

本目录收录了「舜宇工厂 AI 项目立项助手」的全部技能模块（Skill）。每个子文件夹对应一个独立技能，通过 `SKILL.md` 声明元信息与执行逻辑。技能整体遵循 **Agile-Stage-Gate × DMAIC × CRISP-DM** 三轨融合框架，覆盖从场景发现到持续迭代的完整生命周期。

## 全流程总览

```
initiate（总入口路由）
   │
   ├─ Step 0-B  scene-discovery        场景发现：不知道做什么 AI 项目时
   ├─ Step 0-A  scene-prediagnosis     场景预诊断：已有方向，生成资料清单
   ├─ 阶段 1    goal-definition        目标定义：KPI 与基线/目标值
   ├─ 阶段 2    readiness-collection   六维资料收集引导
   ├─ Step 1    readiness-diagnosis    六维准备度 AI 诊断（含目标校准 1.5）
   ├─ Step 2    methodology            调优方法论生成
   ├─ Step 3    action-plan            四阶段行动计划
   ├─ 阶段 4    project-charter        正式立项报告（PID）
   ├─ 阶段 5    execution              执行管理框架（Stage Gate/RACI/风险）
   └─ Step 4    iteration              季度持续迭代更新
```

辅助支持技能：`feishu-guide`（飞书写作规范）、`methodology`（方法论知识库）。

---

## 各子目录作用

### initiate/
**总入口与路由器。** 识别用户当前所处阶段，加载并执行对应的下游技能。本身不做业务分析，只负责引导和分发。

### scene-discovery/
**Step 0-B · 场景发现。** 当用户说"不知道做什么 AI 项目"时进入，通过 4 轮信息收集输出 AI 场景机会地图与 Top 3 推荐。

### scene-prediagnosis/
**Step 0-A · 场景预诊断。** 用户已有具体工序或 AI 方向（视觉检测、良率预测、预测性维护等）时使用，输出定制化的六维资料收集清单。

### goal-definition/
**阶段 1 · 目标定义。** 基于 Hoshin Kanri + SMART + OKR 信心指数，分四步引导用户填写 KPI 矩阵、基线与目标值，并进行雄心度分级（保守/激进/严重错位）。
- `kpi-reference.md`：OEE/DMAIC/TPM/VSM/ISO 50001 等 KPI 参考库。

### readiness-collection/
**阶段 2 · 准备度资料收集指引。** 按"战略/数据/技术/工艺/组织/变革"六维，指导用户如何在飞书云文档中组织材料，强调"写'没有'比不写好"。

### readiness-diagnosis/
**Step 1 + 1.5 · 六维准备度 AI 诊断。** 基于 McKinsey Rewired / BCG AI at Scale / INCIT AIMRI，输出 1-5 级评分、关键瓶颈分析与 AI 启动建议；目标过于激进时自动触发 Step 1.5 目标校准。
- `scoring-criteria.md`：六维评分标准细则。

### methodology/
**Step 2 · 调优方法论生成。** 基于诊断出的优先瓶颈，产出具体算法推荐、最低数据需求、约束下的备选方案及 5M1E 操作步骤。参考 CRISP-DM / DMAIC / TPM 8 柱 / WEF Lighthouse。

### action-plan/
**Step 3 · 四阶段行动计划。** 将方法论拆解为 Stage 1（90 天）→ Stage 2（6 月 PoC）→ Stage 3（12 月规模化）→ Stage 4（24 月持续化），含里程碑、责任方与依赖关系。

### project-charter/
**阶段 4 · 正式立项报告（PID）。** 汇总前期所有输出，生成可提交管理层审批的立项文档，包含执行摘要、六维评级、ROI 三情景模型（保守 60% / 基准 100% / 乐观 120%）与风险评估。参考 PMI PMBOK 7th 与 PRINCE2。

### execution/
**阶段 5 · 执行管理框架。** 提供 Stage Gate 门控评审标准、RACI 矩阵、风险登记册、月报模板及 Stage 1-4 的工作分解，帮助 PM 跟踪执行进展。

### iteration/
**Step 4 · 季度持续迭代更新。** 收集季度行动完成情况、KPI 当前值与新增资料，重新评定六维准备度，更新行动计划优先级，识别新解锁的 AI 方法。遵循 PDCA + 文档飞轮加速机制。


### feishu-guide/
**辅助知识 · 飞书云文档写作规范。** 文档命名规则、文档飞轮理念等参考知识。AI 在指导用户写飞书文档时自动调用，不对用户直接暴露。

---

## 技能之间的衔接关系

- **首次立项流程**：`initiate` → `scene-discovery` 或 `scene-prediagnosis` → `goal-definition` → `readiness-collection` → `readiness-diagnosis` → `methodology` → `action-plan` → `project-charter` → `execution`
- **季度复盘流程**：`iteration` 触发，重跑诊断与计划更新
- **贯穿全流程**：`feishu-guide` 提供写作规范

## 文件约定

- 每个技能目录至少包含一个 `SKILL.md`，在 frontmatter 中声明 `name` / `description` / `disable-model-invocation` / `user-invocable` 等字段。
- 附加的 `.md` 文件（如 `kpi-reference.md`、`scoring-criteria.md`）是被 `SKILL.md` 引用的参考资料。
- 不要在本目录新建与技能无关的散文件。新增技能请按"一技能一目录 + SKILL.md"的结构组织。
