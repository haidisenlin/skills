# Workshop W2：GQM 关键问题识别（问卷式）

**时长**：约 5 分钟
**框架来源**：**GQM = Goal-Question-Metric**（Basili & Rombach 1994）
**交互模式**：**问卷式** —— 按 `SKILL.md` 顶部「🧭 内联问卷生成协议」执行（内联 `deep_research`（深度研究）+ 飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）），用户填好贴回来

## 步骤 1：生成 GQM 问卷 飞书问卷（内联协议）

⚡ **执行内联问卷生成协议**（见 `SKILL.md` 顶部「🧭 内联问卷生成协议」）。差异参数如下，执行完必须调用飞书 MCP `sunny_ai__feishu_create_doc`（创建飞书云文档）并把返回链接发给用户：

```
topic: "GQM (Goal-Question-Metric) 方法论 — 从 Goal 拆解出可回答的关键 Question"
scenario: [用户在准备文档里写的部门年度方针 Goal + 核心 KPI + W1 SIPOC 已识别的 Process 步骤]
industry: 舜宇光学 精密光电制造
doc_name: "[厂区代码]-[姓名]-W2-GQM关键问题-[今日日期]"
purpose: "把模糊的部门方针目标拆成 2-3 个可回答、可行动的 Question，为后续 AI 场景推荐和深度研究提供精准 query"
```

## 步骤 2：将飞书 MCP `sunny_ai__feishu_create_doc` 返回的飞书文档链接发给用户

前面加以下引导语：

```
进入 workshop 第 2 个模块：识别你的关键问题 🔍

你在准备文档里写的 Goal 是：
"{Goal}"

GQM 框架告诉我们：好的 Goal 必须能拆成几个可回答的 Question。
我已经基于 GQM 原版方法论 + 你的 Goal，为你定制了一份飞书问卷。

问卷里会给你 2-3 个参考示例（比如：
 ❌ 反例 Question："怎么提升 FPY？"（这等于把 Goal 重复了一遍，没拆解）
 ✅ 正例 Q1：哪几个工序贡献了 80% 的不良？
 ✅ 正例 Q2：在哪些参数组合下不良率最高？
 ✅ 正例 Q3：同样的物料和参数，为什么不同班次的良率差距 3%？）

请模仿示例，围绕你的 Goal 写 2-3 个可拆解、可行动的 Question。

预计 5 分钟。

━━━ 飞书文档 ━━━
{飞书 MCP sunny_ai__feishu_create_doc 返回的文档链接}
```

## 步骤 3：等用户提交

- ① 用户粘贴飞书文档链接 → 调用 `sunny_ai__feishu_get_doc` 解析
- ② 用户直接粘贴文档内容 → 解析
- ③ 用户说"我直接口述"→ 降级为对话式

## 步骤 4：理解复述 + 追问

基于用户填的内容，复述并检查质量：

```
👌 你的 GQM 关键问题清单：
  • Q1：{用户的 Q1}
  • Q2：{用户的 Q2}
  • Q3：{用户的 Q3}（如果有）

{如果用户的某个 Question 是把 Goal 复述了一遍（如"如何提升良率"），追问：
 "⚠️ 我看你的 Q{N} 是【{Q 原文}】——这个问题答了之后你能直接行动吗？
  如果答案是'再继续想'，那它还不是 Question，再拆一层。"}

这些 Question 后面我会用来做更精准的行业最佳实践研究。我们进入第 3 个模块。
```

## 追问规则

如果用户写的 Question 是把 Goal 复述（如"如何提升良率"），追问："这个问题答了之后你能直接行动吗？如果答案是'再继续想'，那它还不是 Question，再拆一层。"

## 确认完进入 `references/workshop-w3-pain-deep-dive.md`
