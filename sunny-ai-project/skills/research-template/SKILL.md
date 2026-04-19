---
name: research-template
description: （已废弃）原为其他技能调用的方法论研究 + 飞书文档生成代理，现各 skill 已改为内联调用 deep_research + sunny_ai__feishu_create_doc，不再经本 skill 中转。仅作历史参考与降级 fallback。
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc
metadata:
  user-invocable: "false"
  deprecated: "true"
  context: fork
  model: sonnet
  effort: medium
---

# ⚠️ 本 skill 已废弃（2026-04-18）

**新的标准做法**：各业务 skill 的 SKILL.md 顶部定义了「🧭 内联问卷生成协议」，由 skill 直接调用 `deep_research` + `sunny_ai__feishu_create_doc` MCP 完成问卷生成与飞书建档。

**不要再通过 read_file 调用本 skill。** 业务 skill 已内联了差异参数（research_query / topic / scenario / doc_name / purpose / 问卷章节建议），可直接执行。

**保留本文件的原因**：
1. 历史参考 —— 新 skill 的内联协议直接源自本文件的执行步骤 A-E
2. 降级 fallback —— 极少数情况下，若某 skill 尚未完成内联改造，可临时回退至本 skill

---

## 📚 历史执行参考（仅降级时使用）

若确需调用（降级场景），通过上下文接收参数：

- **topic**：问题主题
- **scenario**：用户具体场景
- **industry**：行业类型
- **doc_name**：飞书文档命名
- **purpose**：收集信息的目的

### 执行步骤（已迁移至各业务 skill 的内联协议）

**步骤 1 · 目标分析** — 判断专业领域、适配方法论、关键信息类型。

**步骤 2 · 调用 `deep_research`** — query 的目标是**"向用户提问所用的访谈 / 评估方法论"**，不是"问题的领域知识答案"。构造：
```
[intake 目标中文] + "访谈框架 / 问卷设计 / 结构化评估协议"
+ 英文：[intake goal] + "intake framework / interview protocol / structured questionnaire / assessment checklist"
+ 列举 2-3 个真实方法论名（SIPOC / GQM / CRISP-DM Business Understanding / NIST AI RMF Map / Hoshin X-Matrix / ADKAR / McKinsey Rewired Diagnostic 等）
```

权威来源：McKinsey Rewired / BCG AI at Scale / Deloitte / WEF Lighthouse / INCIT AIMRI / AIAG-VDA FMEA / DAMA-DMBOK / NIST AI RMF / ISO 13053 / 55001 / PMI PMBOK / PRINCE2 / Prosci ADKAR / Kotter / Basili GQM / 工业互联网产业联盟（AII）。

**步骤 3 · 合成 3-5 条方法论洞察**。

**步骤 4 · 生成飞书文档内容**（标准见 `feishu-guide/SKILL.md` 中的"AI 自动生成的飞书文档规范"章节）：

- 字段只用 🟡 推荐填 / ⚪ 选填两档（禁用必填）
- 推荐填 : 选填 ≈ 6:4
- 量化字段 ≥ 50%
- 每章节 ≤ 8 行
- 每字段括号内说明 + 每章节 1-2 行斜体示例
- 末尾「📌 填写说明」章节

**步骤 5 · 调用 `sunny_ai__feishu_create_doc`**：
```
title: [doc_name]
content: [步骤 4 生成的 markdown 全文]
```

失败则降级为输出 markdown 让用户手动建文档。

**回复标准话术**：
```
━━━ 📚 方法论来源 ━━━
[2-3 行，基于 deep_research 结果]

━━━ ✅ 飞书文档已创建 ━━━
文档名称：[doc_name]
文档链接：[MCP 返回链接]

请打开文档，按模板填写后把链接粘贴回对话。

━━━ 💡 填写要点 ━━━
1. [最重要注意事项]
2. [数据不确定时估算 + 标注"(估算)"]
3. [现场来不及填可留空，AI 会标低置信度继续]
```

---

## 迁移提示（若你在维护 skill）

新写 skill 时直接内联这套协议，不要 read_file 本 skill。参考模板：`scene-prediagnosis/SKILL.md` 顶部的「🧭 内联问卷生成协议」章节（步骤 A-F）。
