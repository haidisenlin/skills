# sunny-ai-project

> 舜宇工厂 AI 辅助改善 · 全流程立项助手

[![version](https://img.shields.io/badge/version-1.0.0-blue)](https://www.sunnyoptical.com)
[![author](https://img.shields.io/badge/author-Sunny%20Optical-orange)](https://www.sunnyoptical.com)

---

## 项目简介

`sunny-ai-project` 是面向舜宇制造体系的 **工厂 AI 项目全流程立项助手**，以 Claude Cowork 插件形式运行。它覆盖从"发现 AI 改善机会"到"正式立项并启动执行"的完整链路，帮助工程师和项目经理在没有数据科学背景的情况下，系统性地规划、评估和推进工厂 AI 改善项目。

### 方法论基础

本插件的评估框架和行动设计，综合参考了以下权威方法论：

- **McKinsey Rewired** — 数字化转型能力建设框架
- **BCG AI Manufacturing** — 工厂 AI 落地路径
- **WEF Lighthouse Factory** — 灯塔工厂评估标准
- **INCIT AIMRI** — AI 成熟度分级模型
- **Six Sigma DMAIC** — 数据驱动的持续改进方法
- **TPM / TQM** — 全面生产维护与全面质量管理

---

## 核心能力

整个立项流程分为 7 个阶段，每个阶段对应一个独立技能模块：

| 阶段 | 技能模块 | 功能描述 |
|------|----------|----------|
| 0-B | `scene-discovery` | 从工厂痛点出发，发现最适合 AI 的工序场景，输出 AI 场景机会地图及 Top 3 推荐 |
| 0-A | `scene-prediagnosis` | 对已选定的目标工序做快速预诊断，生成定制化六维资料收集清单 |
| 1 | `goal-definition` | KPI 矩阵定义，设定基线与目标值，包含雄心度分级校验（保守/激进/严重错位）|
| 2 | `readiness-collection` | 逐维度引导用户收集六维准备度飞书文档（战略/数据/技术/工艺/组织/变革）|
| 3 | `readiness-diagnosis` | AI 诊断六维准备度评分（1–5 级），识别关键瓶颈，给出启动建议 |
| 4 | `methodology` | 针对关键瓶颈推荐 AI 算法、最低数据需求、5M1E 操作步骤及约束条件下的备选方案 |
| 5 | `action-plan` | 生成四阶段行动清单（Stage 1 · 90天 → Stage 2 · 6月 → Stage 3 · 12月 → Stage 4 · 24月）|
| 6 | `project-charter` | 生成可提交管理层审批的正式 PID 立项报告，含 ROI 三情景模型与风险评估 |
| 7 | `execution` | 提供 Stage Gate 门控评审标准、RACI 矩阵、风险登记册与月报模板 |
| ∞ | `iteration` | 每季度更新六维评级与行动计划，持续跟踪项目进展 |

此外，`research-template` 作为 Sub-Agent，在各技能遇到需要系统性填写的复杂分析问题时，自动调用行业方法论研究能力并生成飞书文档模板。

---

## 快速开始

### 入口命令

在 Claude Cowork 中运行以下命令启动全流程向导：

```
/sunny-ai-project:initiate
```

助手会询问你当前所处阶段，并自动路由到对应技能模块。

### 直接调用技能

也可以跳过向导，直接调用对应阶段的技能：

```
/sunny-ai-project:scene-discovery      # 从痛点出发发现 AI 机会
/sunny-ai-project:scene-prediagnosis   # 预诊断目标工序，获取资料清单
/sunny-ai-project:goal-definition      # KPI 矩阵 + 目标合理性校准
/sunny-ai-project:readiness-collection # 六维资料收集指导
/sunny-ai-project:readiness-diagnosis  # 六维评分 + 关键瓶颈分析
/sunny-ai-project:methodology          # AI 调优方法论生成
/sunny-ai-project:action-plan          # 四阶段行动清单
/sunny-ai-project:project-charter      # 正式 PID 立项报告生成
/sunny-ai-project:execution            # Stage Gate + RACI + 风险 + 月报
/sunny-ai-project:iteration            # 季度迭代更新
```

---

## 项目结构

```
sunny-ai-project/
├── .claude-plugin/
│   └── plugin.json              # 插件元数据
├── commands/                    # Claude Code 斜杠命令（/command 触发）
│   ├── initiate.md
│   ├── scene-discovery.md
│   ├── scene-prediagnosis.md
│   ├── goal-definition.md
│   ├── readiness-collection.md
│   ├── readiness-diagnosis.md
│   ├── methodology.md
│   ├── action-plan.md
│   ├── project-charter.md
│   ├── execution.md
│   └── iteration.md
├── skills/                      # 技能模块（各阶段核心逻辑）
│   ├── initiate/SKILL.md        # 全流程入口路由
│   ├── scene-discovery/         # 场景发现
│   ├── scene-prediagnosis/      # 场景预诊断
│   ├── goal-definition/         # 目标定义（含 KPI 参考库）
│   ├── readiness-collection/    # 六维资料收集指引
│   ├── readiness-diagnosis/     # 准备度诊断（含评分标准）
│   ├── methodology/             # 调优方法论生成
│   ├── action-plan/             # 四阶段行动计划
│   ├── project-charter/         # 立项报告生成
│   ├── execution/               # 执行管理框架
│   ├── iteration/               # 季度迭代更新
│   ├── feishu-guide/            # 飞书云文档写作规范
│   └── research-template/       # 方法论研究 Sub-Agent
├── agents/
│   └── research-template.md     # Sub-Agent 定义
└── script/
    └── history.md               # 版本历史
```

---

## 典型使用场景

**场景 A：我不知道工厂哪里适合做 AI**

运行 `/sunny-ai-project:scene-discovery`，通过 4 轮信息收集，生成 AI 场景机会地图与 Top 3 推荐。

**场景 B：我已选定工序（如 AOI 视觉检测），想评估可行性**

运行 `/sunny-ai-project:scene-prediagnosis`，获取针对该工序的定制化六维资料收集清单，再进入准备度诊断。

**场景 C：准备度诊断已完成，需要方法论和行动计划**

依次运行 `methodology` → `action-plan`，获得具体 AI 算法推荐和可落地的四阶段行动清单。

**场景 D：项目已运行一段时间，需要季度复盘**

运行 `/sunny-ai-project:iteration`，更新六维评级、跟踪 KPI 变化、解锁新阶段行动。

---

## 六维准备度框架

准备度诊断从以下六个维度对项目可行性进行 1–5 级评分：

| 维度 | 评估重点 |
|------|----------|
| **战略** | AI 项目与公司战略的对齐度，管理层支持力度 |
| **数据** | 历史数据量、质量、标注完整性、可获取性 |
| **技术** | IT/OT 基础设施、算力资源、系统集成能力 |
| **工艺** | 工艺参数可控度、机理可解释性、SOP 完备性 |
| **组织** | 跨部门协作机制、AI 人才储备、培训体系 |
| **变革** | 员工接受度、变革管理成熟度、激励制度 |

---

## 作者与维护

- **组织**：舜宇光学科技（集团）有限公司
- **联系**：hsli@sunnyoptical.com
- **主页**：https://www.sunnyoptical.com
- **版本**：1.0.0
