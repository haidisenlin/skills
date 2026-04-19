---
name: iteration
description: 工厂AI项目季度迭代更新（Step 4）。收集本季度行动完成情况、KPI当前值和新增资料，重新评定六维准备度等级，更新行动计划优先级，识别新解锁的AI方法，并推荐下季度最优先补充的资料。
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc Read Write
metadata:
  disable-model-invocation: "true"
---

# 季度持续迭代更新（Step 4）

**运行原则：** PDCA × 文档飞轮。每季度补充新数据 / 发现，更新飞书文档，触发 AI 重评。文档越丰富，建议越精准。

**参考：** PDCA（Deming）· 敏捷回顾 · PSI 漂移监控 · OKR 季度复盘（John Doerr）

**频率：** 每季度一次；下列情况可提前触发：
- 模型精度下降（PSI > 0.25）
- 重大质量事故
- 关键角色变动（PE/AIE 离职）
- 管理层要求重评

---

## 🧭 内联问卷生成协议（本 skill 通用执行块）

本 skill 所有"问卷式输入点"**直接内联**调用 `deep_research` + `sunny_ai__feishu_create_doc`。

**A · deep_research（必做）** — query 的目标是**"向用户提问所用的访谈/评估方法论"**（SIPOC / GQM / 5W1H / A3 / CRISP-DM Business Understanding / Bernard Marr AI Use Case Canvas / NIST AI RMF Map / Hoshin X-Matrix / AIAG FMEA / Prosci ADKAR / Kotter 8-step / McKinsey Rewired Diagnostic 等），**不是**"问题的领域知识答案"。用户此刻还没给出完整问题，我们要的是"**怎么问才能把信息问全**"的访谈工具，不是"问题怎么解决"的答案。

query 构造：`[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议" + [intake goal 英文] + "intake framework / interview protocol / structured questionnaire / assessment checklist" + 列举 2-3 个真实方法论名`

权威来源：McKinsey Rewired / BCG AI at Scale / WEF Lighthouse / INCIT AIMRI / ISO 13053 / 55001 / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter / Basili GQM。

**B · 合成 3-5 条洞察**。
**C · 生成飞书** — 🟡/⚪；6:4；量化 ≥ 50%；每章节 ≤ 8 行；末尾「📌 填写说明」。
**D · 调 `sunny_ai__feishu_create_doc`**（失败降级）。
**E · 回用户**：来源 + 链接 + 要点 3 条。
**F · 等贴回** → 解析 → 追问 🟡 → 低置信度继续。

---

## 第一步：季度迭代输入收集——执行内联问卷生成协议

说明语：

```
欢迎本季度 AI 项目迭代更新！本次更新将：
✅ 重评六维准备度
✅ 调整剩余行动优先级
✅ 识别新解锁的 AI 方法
✅ 确定下季度最重要的补充工作
```

差异参数：

```
research_query: "AI 项目季度回顾访谈框架 PDCA Plan-Check interview OKR quarterly review protocol Derby Larsen retrospective facilitation 5-step questionnaire manufacturing AI iteration intake 准备度重评访谈 行动复盘问卷"
topic: "工厂 AI 项目季度 PDCA 迭代 · 进展与准备度更新"
scenario: [用户当前进展、已完成行动、KPI 变化]
industry: [用户行业]
doc_name: "[厂区代码]-[项目简称]-季度迭代输入-[YYYYMMDD]"
purpose: "收集本季度行动完成情况、维度升级、KPI 进展，为迭代评估提供输入"
```

**问卷章节建议**：
- 当前 Stage + 距上次评估时间（🟡）
- 本季度已完成行动 & 未完成行动（🟡 各 3-5 条）
- KPI 当前值 vs 基线 vs 目标（🟡 主次 KPI）
- 六维评级变化自评（🟡 6 行，上次 → 本次 → 原因）
- 新增飞书资料（⚪ 列表）
- 新发生的风险 / 阻碍（🟡）
- 关键人员变动（⚪）
- ROI 当前测算（⚪）

话术：

> 这是季度迭代的信息收集。把本季度完成情况、KPI 进展、维度变化填进去。
> 完成后我重评准备度、识别新解锁的 AI 方法、更新下季度行动优先级。

---

## 第二步：迭代报告输出

```
【季度迭代更新报告】
工序：[名]  更新日期：[YYYYMMDD]  距上次：[N] 月
当前 Stage：Stage [N]

━━━ 📊 一、六维准备度重评 ━━━

| 维度 | 上次 | 本次 | 变化 | 关键依据 |
|---|---|---|---|---|
| ①战略价值度 | X | Y | ⬆️+1 / ➡️ / ⬇️ | [主要改变] |
| ②数据准备度 | | | | |
| ③技术互联度 | | | | |
| ④工艺可控度 | | | | |
| ⑤组织能力度 | | | | |
| ⑥管理变革度 | | | | |
| **综合** | | | | |

[维度提升解读]：
  - [维度]：X→Y。原因：...。影响：...

[未变 / 下降说明]：
  - ...

━━━ 🔓 二、新解锁的 AI 方法 ━━━

维度② X→Y 级：
  ✅ 新解锁：[如"可训练集构建，PatchCore 无标注 → YOLOv8 精标注"]

维度③ X→Y：
  ✅ 新解锁：[如"实时推理部署（之前只能离线）"]

[无升级时]：本季度无显著升级，暂无新解锁。建议聚焦最低分维度 [X]，达 Y 级后解锁 [方法]。

━━━ 📋 三、行动计划更新 ━━━

**已完成（✅）：** [列举 + 实际成效]

**阻碍替代方案：**

| 阻碍行动 | 原因 | 替代方案 | 预期 |
|---|---|---|---|
| ... | ... | ... | ... |

**下季度 Top 5（按优先级）：**

| 优先级 | 行动 | 任务 | 责任 | 成功标准 | 资源 |
|---|---|---|---|---|---|
| P1 | | | | | |
| P2-P5 | | | | | |

**里程碑状态：**

| 里程碑 | 计划 | 状态 | 新预计 |
|---|---|---|---|
| ... | ... | 🟢/🟡/🔴 | ... |

━━━ 📈 四、KPI 进展 ━━━

| KPI | 基线 | 当前 | 目标 | 进展率 | 状态 |
|---|---|---|---|---|---|
| ... | | | | | 🟢/🟡/🔴 |

**ROI 更新：**
  - 实际改善年化价值：约 ¥[N] 万
  - 累计成本：约 ¥[N] 万
  - 当前 ROI：正/负/平衡
  - 预计 Stage [N] 末 ROI：[N]%

━━━ 📚 五、下季度最优先补充资料 ━━━

🥇 P1：[资料名]
   维度：[X]  X 级 → Y 级
   补充方式：[如"完成 MSA，出 GR&R 报告"]
   预期影响：[解锁什么]
   飞书：`[工序]_[维度]_[类型]_v1.0`

🥈 P2：[资料名]
🥉 P3：[资料名]

━━━ ⚠️ 六、风险更新 ━━━

**新增 / 升级：**

| 风险 | 可能性 | 影响 | 缓解 |
|---|---|---|---|
| ... | | | |

**已关闭：** [编号] [原因消除]

━━━ 💡 七、总结 ━━━

**整体：** 🟢进展良好 / 🟡基本按计划 / 🔴需重大调整

**最值得庆祝：** [1-2 条]

**最需关注：** [1-2 条]

**下次迭代：** [3 月后具体日期；有重大风险可提前]
```

---

## 信息不足降级

- **缺上次评级** → 声明"重建基线"，按 `readiness-diagnosis` 流程给全新评级
- **KPI 数据不全** → 标"[待补]"，不影响其他
- **某维度无新进展** → 维持上次评级 + 注"本季度无实质变化"
- **行动完成情况不清** → 只分析明确部分，其余"[下次补]"

---

## 完成后衔接

```
💡 文档飞轮提示：本次识别的"下季度补充资料"请在飞书建文档并持续更新。

下一步可：
1. 向 Sponsor 汇报（月报模板：/sunny-ai-project:execution）
2. 六维显著提升 → 重跑 /sunny-ai-project:methodology 获取新方法推荐
3. 综合达 4 级 → 跑 /sunny-ai-project:action-plan 更新完整计划
```
