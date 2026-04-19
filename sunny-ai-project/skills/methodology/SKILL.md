---
name: methodology
description: 工厂AI项目调优方法论生成（Step 2）。当用户已有准备度诊断报告（知道关键瓶颈在哪个维度），需要获得具体AI算法推荐、最低数据需求、约束条件下的备选方案和5M1E操作步骤时使用。完成后衔接行动计划生成。
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc Read Write
---

# AI 调优方法论生成（Step 2）

**方法论来源：** CRISP-DM · Six Sigma DMAIC · TPM 8 柱 · WEF Lighthouse 经验法则（算法选型 / 最低数据要求 / 分阶段部署）

**前提：** 已完成准备度诊断，收到诊断报告（六维评级 + 瓶颈排序）。

---

## 🧭 内联问卷生成协议（本 skill 通用执行块）

本 skill 所有"问卷式输入点"**直接内联**调用 `deep_research` + `sunny_ai__feishu_create_doc`。

**A · deep_research（必做）** — query 的目标是**"向用户提问所用的访谈/评估方法论"**（SIPOC / GQM / 5W1H / A3 / CRISP-DM Business Understanding / Bernard Marr AI Use Case Canvas / NIST AI RMF Map / Hoshin X-Matrix / AIAG FMEA / Prosci ADKAR / Kotter 8-step / McKinsey Rewired Diagnostic 等），**不是**"问题的领域知识答案"。用户此刻还没给出完整问题，我们要的是"**怎么问才能把信息问全**"的访谈工具，不是"问题怎么解决"的答案。

query 构造：`[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议" + [intake goal 英文] + "intake framework / interview protocol / structured questionnaire / assessment checklist" + 列举 2-3 个真实方法论名`

权威来源：McKinsey Rewired / BCG AI at Scale / WEF Lighthouse / INCIT AIMRI / ISO 13053 / 55001 / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter / Basili GQM。

**B · 合成 3-5 条洞察**。
**C · 生成飞书** — 🟡 推荐填 / ⚪ 选填；6:4；量化 ≥ 50%；每章节 ≤ 8 行；末尾加「📌 填写说明」。
**D · 调 `sunny_ai__feishu_create_doc`**（失败降级为贴 markdown）。
**E · 回用户**：方法论来源 + 飞书链接 + 填写要点 3 条。
**F · 等贴回** → 解析 → 追问缺失 🟡 → 低置信度继续。

---

## 第一步：方法论输入汇总——执行内联问卷生成协议

输出方法论前，让用户整理一份输入汇总飞书文档。

差异参数：

```
research_query: "AI 算法选型前置约束访谈框架 瓶颈到算法映射问卷 CRISP-DM modeling phase intake Bernard Marr AI use case canvas post-discovery interview manufacturing bottleneck constraint elicitation questionnaire 5M1E diagnostic interview protocol"
topic: "工厂 AI 项目调优方法论 · 输入汇总（诊断结果 + 约束）"
scenario: [用户已获得的准备度诊断报告 + 优先瓶颈]
industry: [用户行业]
doc_name: "[厂区代码]-[项目简称]-方法论输入-[YYYYMMDD]"
purpose: "整合诊断结论、瓶颈排序、关键约束，为方法论生成提供输入"
```

**问卷章节建议**：六维评级摘要（🟡 复制诊断报告）/ 优先瓶颈 Top 3（🟡）/ 关键约束（停机/预算/权限/OT隔离，🟡）/ 数据现状一句话（🟡）/ 期望改善窗口（🟡 月数）/ 已试过/排除的方法（⚪）/ 竞品/行业案例了解（⚪）。

话术：

> 方法论输入准备。把诊断的六维评级、最关键的 2-3 个瓶颈、约束条件填进去。
> 完成后粘贴链接，我按优先瓶颈生成 AI 算法推荐和 5M1E 操作步骤。

---

## 方法论输出格式

对每个优先瓶颈，按下列格式输出：

```
【[工序] AI 调优方法论】

═══ 维度①：[瓶颈名]（当前 X 级 → 目标 Y 级）═══

推荐方法：[算法名]
选择理由：[2-3 句 · 场景匹配 / 约束适配]

最低数据需求：
  样本量：≥ [N]
  必要字段：[列表]
  历史深度：≥ [X] 月
  视觉：正常 ≥ N 张 / 缺陷 ≥ N 张 / 标注要求 [...]

约束下的调整：[如"OT 隔离 → 边缘计算"；"不可停机 → 在线学习"]

5M1E 操作步骤：
  人：[如"PE 定义参数范围 / AIE 特征工程 / QE 测量系统验证"]
  机：[如"PLC/SCADA 接口确认 / 边缘节点 / 采集频率"]
  料：[如"历史缺陷样本整理 / 标注规范 / 完成 D3/D4 子维度"]
  法：[如"数据清洗→特征选择→模型训练→阈值标定→PoC"]
  环：[如"Stage 1 隔离测试环境，不影响生产"]

研产供销服分工：
  研（主/协）：[职责]
  产（主/协）：[职责]
  供（主/协）：[如"供应商提供原料批次用于 X-Y 关联"]
  销（主/协）：[如"提供客诉数据作 Y 标签"]
  服（主/协）：[如"设备厂商提供 PLC 接口文档"]

预期改善区间：
  主 KPI：+[X%~Y%]（保守 X% / 基准 Y% / 乐观 Z%）
  置信度：低/中/高；理由：[基于数据质量 + 行业案例]

行业参考：
  [1-2 个 WEF Lighthouse 或同类工厂真实案例及改善幅度]

═══ 维度②：... ═══
```

---

## 常用 AI 方法速查

### 视觉 AI（缺陷检测 / 尺寸测量）
- **PatchCore** — 异常检测，仅需正常品训练
- **YOLOv8/v9** — 目标检测，需标注框
- **ResNet / EfficientNet** — 分类任务
- **GAN 数据增强** — 缺陷样本不足时扩充

### 工艺参数优化
- **贝叶斯优化（BO）** — 样本量少，高效探索
- **SHAP 归因** — 识别关键参数影响
- **高斯过程回归（GPR）** — 代理模型
- **Run-to-Run（R2R）** — 批次级反馈控制

### 预测性维护
- **LSTM / Transformer** — 时序异常
- **孤立森林** — 无监督，不需故障标签
- **生存分析** — 预测 RUL
- **频谱 + ML** — 振动 / 声音

### 根因分析
- **决策树 / 规则挖掘** — 可解释性好
- **因果推断（DoWhy）** — 区分相关与因果
- **关联规则（Apriori）** — 缺陷与参数关联

---

## 约束下的方法调整原则

| 约束 | 推荐调整 |
|---|---|
| 不可停机 | 在线学习 / 增量学习；PoC 先在模拟数据验证 |
| OT 隔离 | 边缘节点推理；数据单向 OT→IT；避免云端实时推理 |
| 预算 ≤ 50 万 | 开源模型；避免商业平台授权费；PE+AIE 共建 |
| 参数需客户批准 | 先做预测不执行，记录 AI 建议 vs 实际对比 |
| AIE 不足 | AutoML（H2O/AutoGluon）降门槛；外包建模，PE 做输入 |

---

## 完成后衔接

```
✅ 方法论已生成！有追问可详细解释。

下一步【四阶段行动计划】——把方法论分解为 90 天 / 6 月 / 12 月 / 24 月
可执行清单，含里程碑与责任方。

继续？
```

用户确认 → Read `action-plan/SKILL.md` 执行，传入方法论输出作为上下文。
