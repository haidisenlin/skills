# sunny-ai-project

> 舜宇工厂 AI 辅助改善 · 全流程立项助手

[![version](https://img.shields.io/badge/version-1.0.0-blue)](https://www.sunnyoptical.com)
[![author](https://img.shields.io/badge/author-Sunny%20Optical-orange)](https://www.sunnyoptical.com)
[![platform](https://img.shields.io/badge/platform-Sunny%20Agent-purple)](https://github.com/ZouShuangDian/sunny_agent)

---

## 项目简介

`sunny-ai-project` 是面向舜宇制造体系的**工厂 AI 项目全流程立项助手**，以 [Sunny Agent](https://github.com/ZouShuangDian/sunny_agent) 插件形式运行。覆盖从"发现 AI 改善机会"到"正式立项并启动执行"的完整链路，帮助工程师和项目经理在没有数据科学背景的情况下，系统性地规划、评估和推进工厂 AI 改善项目。

### 方法论基础

| 领域 | 方法论 |
|------|--------|
| 数字化转型 | McKinsey Rewired · BCG AI at Scale · Deloitte AI Maturity |
| 灯塔工厂 | WEF Lighthouse Factory · INCIT AIMRI 成熟度模型 |
| 持续改善 | Six Sigma DMAIC · TPM/TQM · SMED · Hoshin Kanri |
| 项目管理 | PMI PMBOK 7th · PRINCE2 PID · Robert Cooper Agile-Stage-Gate |
| 数据治理 | DAMA-DMBOK · Data Readiness Levels (Lawrence 2017) · ISO 25012 |
| 变革管理 | Prosci ADKAR · Kotter 8-step · McKinsey Influence Model |
| AI 用例设计 | Bernard Marr AI Use Case Canvas · CRISP-DM · NIST AI RMF |

---

## 环境要求

| 依赖 | 说明 |
|------|------|
| **Sunny Agent** | 需先部署 [sunny-agent](https://github.com/ZouShuangDian/sunny_agent) 后端服务（Python 3.10+ / FastAPI / PostgreSQL / Redis） |
| **飞书 MCP**（可选）| `sunny_ai__feishu_create_doc` — 自动创建飞书云文档。未配置时降级为 Markdown 输出 |
| **Deep Research MCP**（可选）| `deep_research` — 行业方法论深度研究。未配置时降级为 WebSearch |

> 插件本身无外部依赖（无额外 Python 包/数据库），纯 Markdown 提示词驱动。

---

## 安装与部署

### 前置：启动 Sunny Agent

确保 sunny-agent 后端服务已正常运行：

```bash
cd /Users/lihaisen/PycharmProjects/sunny-agent

# 启动基础设施（PostgreSQL / Redis / Milvus）
docker compose -f infra/docker-compose.yml up -d

# 启动应用
python -m app.main
```

### 方式一：作为 Sunny Agent 插件部署

将本仓库中的 `sunny-ai-project/` 目录部署到 Sunny Agent 的插件卷挂载目录下：

```bash
# 克隆本仓库
git clone <repo-url> sunny-skills
cd sunny-skills

# 将插件目录复制到 sunny-agent 的插件存储路径
# （具体路径取决于 sunny-agent 的 VOLUME_ROOT 配置）
cp -r sunny-ai-project/ <VOLUME_ROOT>/plugins/<user_id>/sunny-ai-project/
```

然后在 Sunny Agent 数据库中注册插件和命令（参考 `sunny-agent/app/plugins/service.py` 中的 `plugins` 和 `plugin_commands` 表结构）。

### 方式二：开发环境本地加载

在 sunny-agent 项目中，将 `sunny-ai-project/` 放入内置插件目录：

```bash
cp -r sunny-ai-project/ /Users/lihaisen/PycharmProjects/sunny-agent/app/plugins/builtin_plugins/sunny-ai-project/
```

插件元数据定义在 `sunny-ai-project/.claude-plugin/plugin.json`。

### 插件目录结构要求

Sunny Agent 的插件加载机制要求以下标准结构：

```
sunny-ai-project/
├── .claude-plugin/
│   └── plugin.json        # 插件元数据（name / description / version）
├── commands/               # 斜杠命令定义（/command 触发）
│   ├── initiate.md
│   ├── scene-discovery.md
│   └── ...
└── skills/                 # 技能核心逻辑（SKILL.md）
    ├── initiate/SKILL.md
    ├── scene-prediagnosis/SKILL.md
    └── ...
```

---

## 快速开始

### 一句话启动

在 Sunny Agent 对话中输入：

```
/sunny-ai-project:initiate
```

助手会显示欢迎词和阶段选择菜单（A-H），根据你的选择自动路由到对应技能。

### 典型使用场景

| 你的情况 | 推荐命令 | 说明 |
|----------|----------|------|
| 不知道工厂哪里适合做 AI | `/sunny-ai-project:scene-discovery` | 通过 Workshop 模式发现 AI 机会，输出场景机会地图 + Top 3 推荐 |
| 已选定工序，想评估可行性 | `/sunny-ai-project:scene-prediagnosis` | 生成定制化六维资料收集清单 |
| 资料收集好了，要诊断评分 | `/sunny-ai-project:readiness-diagnosis` | 六维 1-5 级评分 + 瓶颈分析 + 启动建议 |
| 诊断完了，要方法论和计划 | `/sunny-ai-project:methodology` | 基于瓶颈推荐 AI 算法 + 行动步骤 |
| 要提交管理层审批 | `/sunny-ai-project:project-charter` | 生成完整 PID 立项报告（含 ROI 三情景） |
| 项目已在跑，季度复盘 | `/sunny-ai-project:iteration` | 更新六维评级 + 调整行动计划 |

---

## 全流程概览

```
┌─────────────────────────────────────────────────────────────────┐
│                     全流程 · 7 个阶段                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐     ┌──────────────────┐                      │
│  │ 0-B 场景发现  │────▶│ 0-A 场景预诊断    │                      │
│  │ scene-       │     │ scene-           │                      │
│  │ discovery    │     │ prediagnosis     │                      │
│  └──────────────┘     └───────┬──────────┘                      │
│                               │                                 │
│  ┌────────────────────────────▼──────────────────────────────┐  │
│  │          1️⃣  准备度评估（三步走）                           │  │
│  │                                                           │  │
│  │  goal-definition ──▶ readiness-collection ──▶ readiness-  │  │
│  │  KPI 矩阵定义        六维资料收集指引         diagnosis   │  │
│  │                                              六维诊断     │  │
│  └────────────────────────────┬──────────────────────────────┘  │
│                               │                                 │
│                  ┌────────────▼────────────┐                    │
│                  │ 2️⃣  methodology          │                    │
│                  │ 调优方法论（AI 算法推荐）  │                    │
│                  └────────────┬────────────┘                    │
│                               │                                 │
│                  ┌────────────▼────────────┐                    │
│                  │ 3️⃣  action-plan          │                    │
│                  │ 四阶段行动计划            │                    │
│                  └────────────┬────────────┘                    │
│                               │                                 │
│                  ┌────────────▼────────────┐                    │
│                  │ 4️⃣  project-charter      │                    │
│                  │ 正式 PID 立项报告         │                    │
│                  └────────────┬────────────┘                    │
│                               │                                 │
│                  ┌────────────▼────────────┐                    │
│                  │ 5️⃣  execution            │                    │
│                  │ 执行管理框架              │                    │
│                  └────────────┬────────────┘                    │
│                               │                                 │
│                  ┌────────────▼────────────┐                    │
│                  │ 🔄  iteration            │                    │
│                  │ 季度迭代更新              │◀──── 每季度循环     │
│                  └────────────────────────┘                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 各技能详细说明

### 0-B. scene-discovery — 场景发现

**适用场景**：不知道工厂哪里适合做 AI，或参加舜宇 AI 场景发现 Workshop。

**执行流程**：
1. 发送会前准备清单（系统清单 / 产线布局 / 损失数据等）
2. 开场问答（行业 + 最大痛点）
3. 4 个 AI 主导的 Workshop 互动模块（SIPOC / GQM / 痛点深挖 / 利益相关方对齐）
4. 3 轮深度信息收集
5. 输出 AI 场景机会地图 + Top 3 推荐 + Bernard Marr AI Use Case Canvas

**输出物**：场景机会地图（飞书文档）、Top 3 AI 场景推荐及优先级排序。

---

### 0-A. scene-prediagnosis — 场景预诊断

**适用场景**：已有目标工序（如"AOI 视觉检测"、"良率预测"），需要知道要准备哪些资料。

**执行流程**：
1. 基本信息收集（工序 / 产品 / 痛点 / KPI）— 生成飞书文档
2. 六维资料需求生成 — **每个维度独立**调用 deep_research + 飞书文档
3. 输出定制化的六维资料收集清单

**输出物**：6 份飞书文档（每维度一份），包含需收集的具体数据项和填写指引。

**六维维度**：

| 维度 | 评估重点 | 示例问题 |
|------|----------|----------|
| ①战略价值度 | AI 项目与公司战略的对齐度 | Sponsor 是谁？OKR 关联？ROI 预期？ |
| ②数据准备度 | 历史数据量、质量、可获取性 | 多少条记录？X-Y 关联率？GR&R？ |
| ③技术互联度 | IT/OT 基础设施和集成能力 | PLC 接口？MES 版本？数据同步频率？ |
| ④工艺可控度 | 工艺参数可控度和机理可解释性 | SPC 在用？关键参数已识别？SOP 完备？ |
| ⑤组织能力度 | AI 人才储备和跨部门协作 | 有 AIE？IT 支持力度？培训体系？ |
| ⑥管理变革度 | 员工接受度和变革管理成熟度 | 班长态度？审批周期？精益文化？ |

---

### 1. goal-definition — 目标定义

**适用场景**：已确认 AI 应用场景，需要设定具体 KPI 目标。

**执行流程**：
1. deep_research 研究 KPI 设定方法论（Hoshin Kanri X-Matrix / WEF Lighthouse / OKR）
2. 生成 KPI 矩阵飞书文档（主 KPI / 次 KPI / 基线 / 目标 / 年化价值）
3. 目标合理性自检（基础核查 + 雄心度分级 + OKR 信心指数）
4. 约束条件与项目背景收集

**雄心度分级**：

| 等级 | 改善幅度 | 含义 |
|------|----------|------|
| 🟢 保守 | < 10% | 稳妥，现有能力可达 |
| 🟡 适中 | 10-25% | 有挑战但合理 |
| 🟠 激进 | 25-40% | 需要显著投入 |
| 🔴 变革性 | > 40% | 需要根本性变化，触发校准 |

---

### 2. readiness-collection — 六维资料收集

**适用场景**：拿到预诊断清单后，逐维度引导收集飞书文档。

引导用户按六维度分别填写飞书文档，每个维度有具体的数据项清单和填写指引。

---

### 3. readiness-diagnosis — 准备度诊断

**适用场景**：六维文档已准备好，需要获得诊断评分和启动建议。

**执行流程**：
1. AI 自动整合前序输出（KPI 矩阵 + 六维飞书文档）
2. 用户补充增量信息（完整度自评 / 最担心维度 / 近期异常）
3. 输出六维评级报告（每维度 1-5 级 + 置信度 + 依据 + 缺口）
4. 5M1E 根因分解（最关键的 2-3 个瓶颈）
5. 资料补充建议（P1/P2/P3 优先级）
6. **条件触发**：目标-现实校准（雄心度 🟠/🔴 或 OKR 信心 ≤ 4 时自动触发）

**评级标准概要**：

| 综合评级 | 含义 |
|----------|------|
| ≥ 3.0 级 | 达到启动门槛，可启动 PoC |
| 2.5-3.0 级 | 条件启动，需 Stage 1 前置建设 |
| < 2.5 级 | 建议先完成前置工作 |

---

### 4. methodology — 调优方法论

**适用场景**：已有诊断报告，需要获得具体 AI 算法推荐。

**输出内容**（按瓶颈维度分别输出）：
- 推荐算法/技术栈（主推 + 备选）
- 最低数据需求
- 约束适配方案
- 5M1E 操作步骤（人/机/料/法/测/环）
- 预期改善和 ROI 估算

---

### 5. action-plan — 四阶段行动计划

**适用场景**：方法论已定，需要可执行的行动清单。

**四阶段结构**：

| 阶段 | 时间 | DMAIC | 核心目标 |
|------|------|-------|----------|
| Stage 1 | 90 天 | Define | 数据基础 + Quick Wins |
| Stage 2 | 6 月 | Measure + Analyze | PoC 验证 + Gate 1 |
| Stage 3 | 12 月 | Improve | 规模化推广 + Gate 2 |
| Stage 4 | 24 月 | Control | 制度化 + 横向复制 |

每个行动项包含：任务描述 / 责任方 / 成功标准 / 资源估算 / 前置依赖。

---

### 6. project-charter — 正式立项报告（PID）

**适用场景**：需要生成可提交管理层审批的正式文件。

**报告十章结构**：
1. 执行摘要（核心命题 + 关键数字 + 立项理由）
2. 项目背景与战略意义（痛点量化 + 不改善代价）
3. 六维准备度摘要
4. 目标与范围（SMART + 范围内外 + 约束）
5. 技术方案概述（AI 选型 + 架构 + 数据治理 + 自制/外购）
6. 实施计划（里程碑摘要）
7. 项目组织与治理（核心角色 + Stage Gate）
8. 预算（范围估算，标"待财务确认"）
9. 风险评估（Top 5 + 止损条件）
10. 投资效益 ROI（三情景：保守 60% / 基准 100% / 乐观 120%）

---

### 7. execution — 执行管理框架

**适用场景**：项目已立项通过，进入执行阶段。

**输出物**：Stage Gate 门控评审标准 / RACI 矩阵 / 风险登记册 / 月报模板 / 各 Stage 工作分解。

---

### 🔄 iteration — 季度迭代更新

**适用场景**：项目运行中，需要季度复盘。

收集本季度行动完成情况和 KPI 当前值，重新评定六维准备度，更新行动计划优先级，识别新解锁的 AI 方法。

---

## 命令速查表

```
/sunny-ai-project:initiate              全流程入口（自动判断阶段）
/sunny-ai-project:scene-discovery        场景发现 Workshop
/sunny-ai-project:scene-prediagnosis     场景预诊断 → 六维资料清单
/sunny-ai-project:goal-definition        KPI 矩阵 + 目标合理性校准
/sunny-ai-project:readiness-collection   六维资料收集指导
/sunny-ai-project:readiness-diagnosis    六维评分 + 瓶颈分析
/sunny-ai-project:methodology            AI 调优方法论生成
/sunny-ai-project:action-plan            四阶段行动清单
/sunny-ai-project:project-charter        正式 PID 立项报告
/sunny-ai-project:execution              执行管理框架
/sunny-ai-project:iteration              季度迭代更新
```

---

## 内联问卷生成协议

每个 skill 的用户输入点均采用统一的**内联问卷生成协议**，流程如下：

```
A. deep_research（必做）
   ↓  搜索"怎么问才能把信息问全"的访谈方法论
B. 合成 3-5 条洞察
   ↓
C. 生成飞书文档内容
   ↓  🟡 推荐填 / ⚪ 选填，6:4 比例，量化 ≥ 50%
D. 调用 sunny_ai__feishu_create_doc 创建飞书文档
   ↓  失败降级为贴 Markdown
E. 回用户：方法论来源 + 飞书链接 + 填写要点
   ↓
F. 用户填写回传 → 解析 → 追问缺失 → 继续流程
```

> deep_research 的 query 方向始终是**访谈方法论**（"怎么问"），而非"问题怎么解决"。这是本插件的核心设计原则。

---

## 自动化测试

项目内置自动化 Skill 测试框架，位于 `sunny-ai-project/test-runs/`。

### 测试机制

- **角色池**：`personas.md` 定义 10 个舜宇角色（覆盖中山/余姚/杭州/上海/越南/印度等厂区，涵盖 QE/PE/EE/CI/IT/RD/MFG/战略/SQE/EHS 职能）
- **轮换规则**：`文件数 mod 10` 选择角色，确保全覆盖
- **测试流程**：
  1. WebSearch 研究角色行业背景 + 舜宇公开信息（Ground Truth）
  2. 扮演角色依次跑 7 个 skill（initiate → project-charter）
  3. 每个 skill 做 ≥ 3 次 WebSearch，模拟 deep_research + 飞书文档
  4. 末尾输出整体评估（✅ 符合预期 / ❌ 发现问题 / ⚠️ 可改进）

### 运行测试

在 Sunny Agent 对话中执行：

```
Read sunny-ai-project/test-runs/runner-prompt.md 并按步骤 1-6 执行
```

### 查看结果

测试日志命名格式：`YYYY-MM-DD-HH-MM-<角色slug>.md`

```bash
ls sunny-ai-project/test-runs/*.md   # 查看所有测试日志
```

关注每个日志末尾的「整体评估」段落，特别是 ❌ 发现问题。

### v2 质量标准

| 指标 | 最低要求 |
|------|----------|
| WebSearch 总次数 | ≥ 3 次/skill（Ground Truth 除外） |
| 飞书文档表格 | ≥ 3 张表，每张 ≥ 15 行 |
| 六维独立展开 | 每维度独立 deep_research + 独立飞书文档 |
| 日志行数 | ≥ 1500 行 |
| query 方向 | 全部为"访谈方法论型" |

---

## 项目结构

```
sunny-skills/
├── README.md                          ← 本文件
└── sunny-ai-project/                  ← Sunny Agent 插件目录
    ├── .claude-plugin/
    │   └── plugin.json                # 插件元数据
    ├── commands/                       # 斜杠命令定义（/command 触发）
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
    ├── skills/                         # 技能核心逻辑
    │   ├── initiate/SKILL.md           # 全流程入口路由
    │   ├── scene-discovery/            # 场景发现（含 Workshop 资料）
    │   │   ├── SKILL.md
    │   │   ├── assets/intake-template.md
    │   │   └── references/             # 12 份 Workshop 参考文档
    │   ├── scene-prediagnosis/SKILL.md # 场景预诊断
    │   ├── goal-definition/            # 目标定义
    │   │   ├── SKILL.md
    │   │   └── references/kpi-reference.md
    │   ├── readiness-collection/SKILL.md
    │   ├── readiness-diagnosis/        # 准备度诊断
    │   │   ├── SKILL.md
    │   │   └── references/scoring-criteria.md
    │   ├── methodology/SKILL.md        # 调优方法论
    │   ├── action-plan/SKILL.md        # 行动计划
    │   ├── project-charter/SKILL.md    # 立项报告
    │   ├── execution/SKILL.md          # 执行管理
    │   ├── iteration/SKILL.md          # 季度迭代
    │   └──feishu-guide/SKILL.md       # 飞书写作规范
    │    
    ├── agents/
    │   └── research-template.md        # Sub-Agent 定义
    ├── test-runs/                      # 自动化测试日志
    │   ├── README.md
    │   ├── runner-prompt.md            # 测试执行提示词
    │   ├── personas.md                 # 10 个测试角色
    │   └── *.md                        # 测试日志文件
    └── script/
        └── history.md                  # 版本历史
```

---

## 开发与扩展

### 添加新技能

1. 在 `skills/` 下创建新目录，编写 `SKILL.md`（遵循 frontmatter 格式：name / description / allowed-tools）
2. 在 `commands/` 下创建对应的 `.md` 命令文件
3. 更新 `initiate` 的路由逻辑
4. 在 `test-runs/personas.md` 中验证新技能是否被覆盖

### SKILL.md 格式

```yaml
---
name: skill-name
description: 一句话描述何时使用此技能
allowed-tools: deep_research WebFetch sunny_ai__feishu_create_doc Read Write
---

# 技能标题

## 内联问卷生成协议
[统一的 deep_research + 飞书文档协议]

## 执行步骤
[具体逻辑]

## 输出格式
[标准化输出模板]

## 完成后衔接
[下一步引导]
```

### 修改评分标准

六维评分标准位于 `skills/readiness-diagnosis/references/scoring-criteria.md`，KPI 参考库位于 `skills/goal-definition/references/kpi-reference.md`。

---

## FAQ

**Q: 没有飞书 MCP 能用吗？**
A: 可以。每个 skill 都有降级机制——飞书调用失败时直接输出 Markdown 格式内容，用户可自行复制到任何文档工具。

**Q: 必须按顺序走完全流程吗？**
A: 不必须。每个 skill 可独立调用，但建议至少完成 scene-prediagnosis → readiness-diagnosis → methodology 这条核心路径，因为后续 skill 会引用前序输出。

**Q: 支持哪些 AI 场景？**
A: 不限定具体场景。已测试覆盖：视觉检测（AOI）、良率预测、参数优化、预测性维护、排程优化、换型预测、SPC 异常检测、XR 视觉等。方法论推荐会根据六维诊断结果动态生成。

**Q: 一次对话能跑完全流程吗？**
A: 可以但不建议。每个 skill 涉及信息收集和飞书文档交互，建议每次聚焦 1-2 个阶段，利用 initiate 的历史进度检测功能续接。

**Q: 如何修改测试角色？**
A: 编辑 `test-runs/personas.md` 中的角色表即可。格式为：编号 / slug / 厂区 / BU 产品 / 职能 / 核心 KPI。

**Q: 和直接用 Claude Code 有什么区别？**
A: 本插件设计为在 Sunny Agent 平台上运行。Sunny Agent 提供了用户管理、会话持久化、插件注册、飞书集成等企业级能力，插件的 skills 和 commands 通过 Sunny Agent 的插件系统加载和路由。

---

## 作者与维护

- **组织**：舜宇光学科技（集团）有限公司
- **联系**：hsli@sunnyoptical.com
- **主页**：https://www.sunnyoptical.com
- **版本**：1.0.0
