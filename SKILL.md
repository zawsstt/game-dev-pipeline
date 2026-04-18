---
name: game-dev-pipeline
description: >
  通用游戏工业化 AI 全流程工作室管线（Autonomous Multi-Agent Game Production Pipeline）。
  适用于任意品类、任意引擎的游戏项目。Master Agent 根据品类标签动态派生专属
  Sub-Agent 团队（固定层 12 个，覆盖策划/美术/音效/特效/动画/关卡/文案/技术/数据/发行/本地化
  + 动态层 2-5 个品类专属 Agent）。每个 Sub-Agent 强制执行 SSP v1.0 协议
  （搜-研-产三步闭环），禁止跳过搜索直接生成。
  Master Agent 在关键节点触发用户决策门（User Gate），用户确认后派发第二层实现 Ticket，
  Sub-Agent 可自主拆解子任务执行；用户否决时精准修订受影响模块，不推倒重来。
  包含完整 System Prompt 体系、品类→Agent 映射表、SSP v1.0 协议、用户决策门协议。
  Use when: 游戏立项 / 竞品调研 / 游戏数值设计 / 战斗系统 / 游戏经济系统 /
  UI 架构 / 音效规格 / 特效规格 / 动画规格 / 关卡设计 / 游戏文案 /
  游戏留存设计 / 心流设计 / Roguelike 随机性 / 养成曲线 / 发行上线规划 /
  game design pipeline / game pre-production / game balance /
  game UI UX / game economy / game audio VFX animation /
  game level design / game narrative / game analytics / game publishing /
  任意游戏想法→完整工业化制作流程.
---

# 游戏工业化预研 AI 管线 · SSP v1.0

> **通用 Skill，适配任意游戏品类与引擎。**
> Master Agent 根据品类自动派生专属 Sub-Agent 团队，
> 所有 Agent 强制执行"搜-研-产"SSP v1.0 闭环协议。

---

## 快速启动

**全流程激活（推荐）：**
```
[系统] 你是游戏工业化 Master Agent。
请按 master-agent.md 八阶段 SOP 处理以下游戏想法：

{用户游戏想法}

已知项目上下文（可选）：
- 品类：{e.g. 策略/SLG}
- 引擎：{e.g. Unity 2022 LTS}
- 团队规模：{e.g. 5人}
- 目标平台：{e.g. PC Steam}
```

**单 Agent 直接执行：**
```
[系统] 你是 {Agent名称}，执行以下 Ticket。
强制遵循 protocol.md 中的 SSP v1.0 协议（Step 1 搜索优先）。

{TICKET 内容}
```

---

## 八阶段 SOP 总览

```
阶段一  The Clarifier         市场搜索 + 结构化反问  ──→  项目立项书 (Pitch Deck)
  ↓
阶段二  Deep Researcher        竞品搜索 + 评论抓取    ──→  反共识竞品拆解报告
  ↓
阶段三  Project Orchestrator   品类识别 + 动态派生    ──→  专属 Sub-Agent 列表 + Tickets
  ↓
阶段四  运控层（看板 + 播报 + 交付）
        ├── [固定A] 进程感知 Agent   PACING  策划：节奏/里程碑
        ├── [固定B] UI/UX Agent      UX      策划：交互状态机
        ├── [固定C] 美术总监 Agent   ART     美术：视觉风格/资源规格
        ├── [固定E] 音效设计 Agent   AUDIO   音效：BGM分轨/SFX清单/自适应音乐
        ├── [固定F] VFX 特效 Agent   VFX     美术：粒子/屏幕特效/GPU预算
        ├── [固定G] 动画 Agent       ANIM    美术：角色动作/UI动效/状态机
        ├── [固定H] 关卡设计 Agent   LEVEL   策划：关卡骨架/空间引导/验收标准
        ├── [固定I] 文案/叙事 Agent  COPY    策划：文案风格/UI文案/剧情框架
        ├── [固定L] 本地化 Agent     L10N    策划：多语言/文化适配（可选）
        ├── [固定D] 技术评估 Agent   TECH    技术：可行性/性能预算 ★批次B串行
        ├── [固定J] 数据埋点 Agent   DATA    数据：指标/埋点清单   ★批次C串行
        ├── [固定K] 发行运营 Agent   PUBLISH 运营：上线Checklist   ★批次C串行
        └── [动态] 品类专属 Agent × 2-5
        Master Agent：任务看板 / 主动巡查 / 收单处理 / 打回/通过/降级
  ↓
阶段五  QA & Master Review     Post-mortem 核查      ──→  集成一致性确认报告
  ↓
阶段六  Release Planner        版本分层              ──→  MVP/v1.0/v1.x 路线图 + 里程碑  ← 新增
  ↓
阶段七  QA Agent 启动          测试用例生成           ──→  测试计划文档（每条设计结论→可执行用例）← 新增
  ↓
阶段八  Risk Register          风险汇总              ──→  风险登记簿（P0/P1/P2 分级管控）← 新增
```

---

## 动态 Sub-Agent 派生机制（核心特性）

Master Agent 从立项书提取品类标签后，查询映射表自动组建 Sub-Agent 团队：

**执行批次说明：**
- **批次 A（并行）**：所有策划/美术/内容设计 Agent 同时激活
- **批次 B（串行）**：技术评估 Agent 在批次 A 全部 PASS 后激活，对所有产出做可行性审查
- **批次 C（串行）**：版本路线图完成后激活，依赖阶段六输出

| 层级 | Agent | 代号 | 激活条件 | 执行批次 |
|------|------|------|---------|---------|
| 固定层 A | 进程感知 Agent | PACING | 任何品类，强制 | 批次 A |
| 固定层 B | UI/UX Agent | UX | 任何品类，强制 | 批次 A |
| 固定层 C | 美术总监 Agent | ART | 任何品类，强制 | 批次 A |
| 固定层 D | **技术评估 Agent** | TECH | 任何品类，强制 | **批次 B（串行）** |
| 固定层 E | 音效设计 Agent | AUDIO | 任何品类，强制 | 批次 A |
| 固定层 F | VFX 特效 Agent | VFX | 任何品类，强制 | 批次 A |
| 固定层 G | 动画 Agent | ANIM | 任何品类，强制 | 批次 A |
| 固定层 H | 通用关卡设计 Agent | LEVEL | 任何品类，强制 | 批次 A |
| 固定层 I | 文案/叙事 Agent | COPY | 任何品类，强制 | 批次 A |
| 固定层 J | **数据埋点 Agent** | DATA | 任何品类，强制 | **批次 C（串行）** |
| 固定层 K | **发行运营 Agent** | PUBLISH | 任何品类，强制 | **批次 C（串行）** |
| 固定层 L | 本地化 Agent【可选】| L10N | 默认激活，可手动停用 | 批次 A |
| 动态层 | 数值战斗 Agent | BALANCE | 策略/SLG | 批次 A |
| 动态层 | 经济系统 Agent | ECON | 策略/SLG / 模拟经营 | 批次 A |
| 动态层 | 成长曲线 Agent | GROWTH | 养成类 | 批次 A |
| 动态层 | 情感联结 Agent | EMOTION | 养成类 | 批次 A |
| 动态层 | 留存钩子 Agent | RETAIN | 养成类 | 批次 A |
| 动态层 | 心流设计 Agent | FLOW | 休闲/超休闲 | 批次 A |
| 动态层 | 关卡编辑 Agent | LEDIT | 休闲/超休闲 / Puzzle | 批次 A |
| 动态层 | 即时反馈 Agent | JUICE | 休闲/超休闲 / 动作 | 批次 A |
| 动态层 | 随机性设计 Agent | RNG | Roguelike | 批次 A |
| 动态层 | Build 多样性 Agent | BUILD | Roguelike | 批次 A |
| 动态层 | 死亡体验 Agent | DEATH | Roguelike | 批次 A |
| 动态层 | 分支剧情 Agent | BRANCH | 叙事/AVG | 批次 A |
| 动态层 | 情绪节奏 Agent | MOOD | 叙事/AVG | 批次 A |
| 动态层 | 世界构建 Agent | WORLD | 叙事/AVG | 批次 A |
| 动态层 | 难度曲线 Agent | DIFF | Puzzle | 批次 A |
| 动态层 | 解法空间 Agent | SOLVE | Puzzle | 批次 A |
| 动态层 | 经济循环 Agent | LOOP | 模拟经营 | 批次 A |
| 动态层 | 空间叙事 Agent | SPACE | 模拟经营 | 批次 A |
| 动态层 | 手感设计 Agent | FEEL | 动作 | 批次 A |
| 动态层 | 敌人行为 Agent | ENEMY | 动作 | 批次 A |

> 完整 System Prompt 见 [sub-agents.md](sub-agents.md)

---

## 能力接口（所有 Agent 可调用）

| 接口 | 说明 |
|------|------|
| `Web_Search(query, domain_filter)` | 定向搜索，支持限定 GitHub/Reddit/Steam/GDC 等 |
| `Scrape_Comments(url)` | 提取指定页面用户评论，识别痛点与爽点 |
| `Fetch_OpenSource_Framework(repo_url)` | 从 GitHub 提取仓库架构摘要 |

---

## 文件索引

| 文件 | 内容 |
|------|------|
| [master-agent.md](master-agent.md) | Master Agent 完整 System Prompt（八阶段 SOP + 12个固定层Agent动态派生 + 用户决策门GATE + 第二层实现派单 + 死循环防护） |
| [sub-agents.md](sub-agents.md) | 固定层 + 动态层全品类 Sub-Agent System Prompts |
| [protocol.md](protocol.md) | SSP v1.0 独立协议文档（所有 Agent import/引用） |
| [reference.md](reference.md) | 通用游戏设计速查手册（品类惯例 + 数值参考） |

