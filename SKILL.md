---
name: game-dev-pipeline
description: >
  通用游戏工业化 AI 全流程工作室管线 v2.0（Autonomous Multi-Agent End-to-End Game Production Pipeline）。
  适用于任意品类、任意引擎的游戏项目。v2.0 新增：平台确认门(GATE-P)、环境配置门(GATE-S)、
  成本估算与路由选择门(GATE-C·P0级)、AIGC资产实际生成执行(图像/音频/特效)、DEV研发主程Agent
  (工程搭建+代码编写+自修复)、集成打包交付阶段。
  Master Agent 根据品类标签动态派生专属 Sub-Agent 团队（固定层 13 个，含新增 DEV Agent，
  覆盖策划/美术/音效/特效/动画/关卡/文案/技术/数据/发行/本地化/研发
  + 动态层 2-5 个品类专属 Agent）。每个 Sub-Agent 强制执行 SSP v1.1 协议
  （搜-研-产三步闭环 + AIGC执行协议 + 代码编写协议），禁止跳过搜索直接生成。
  Master Agent 在关键节点触发完整 GATE 链（GATE-P→S→C→0→1→2→3→4→C2(可选)→5→6→7），
  用户确认后派发第二层实现 Ticket，Sub-Agent 可自主拆解子任务执行；
  用户否决时精准修订受影响模块，不推倒重来。
  包含完整 System Prompt 体系、品类→Agent 映射表、SSP v1.1 协议、
  AIGC 资产生成执行规范、代码编写执行规范、运行时日志目录体系。
  Use when: 游戏立项 / 竞品调研 / 游戏数值设计 / 战斗系统 / 游戏经济系统 /
  UI 架构 / 音效规格 / 特效规格 / 动画规格 / 关卡设计 / 游戏文案 /
  游戏留存设计 / 心流设计 / Roguelike 随机性 / 养成曲线 / 发行上线规划 /
  AIGC 游戏资产生成 / 游戏工程代码生成 / 自动化游戏制作 / 零代码游戏开发 /
  game design pipeline / game pre-production / game balance /
  game UI UX / game economy / game audio VFX animation /
  game level design / game narrative / game analytics / game publishing /
  AIGC game assets / game code generation / end-to-end game production /
  任意游戏想法→完整工业化制作流程→可运行游戏工程.
---

# 游戏工业化 AI 管线 v2.0 · SSP v1.1 · 端到端全自动生产

> **通用 Skill，适配任意游戏品类与引擎。**
> Master Agent 根据品类自动派生专属 Sub-Agent 团队，
> 所有 Agent 强制执行"搜-研-产"SSP v1.1 闭环协议。
> **v2.0 核心升级**：从"预研文档生成"跨越为"端到端全自动游戏生产"。
> 新增 GATE-S（环境配置门）、GATE-C（成本估算门·P0）、AIGC 资产实际执行、DEV 研发主程 Agent。

---

## 快速启动

**全流程激活（推荐，v2.0 端到端生产模式）：**
```
[系统] 你是游戏工业化 Master Agent v2.0。
请按 master-agent.md 十一阶段 SOP（含 GATE-P/S/C 前置门）处理以下游戏想法：

{用户游戏想法}

已知项目上下文（可选）：
- 品类：{e.g. 策略/SLG}
- 引擎：{e.g. Unity 2022 LTS}（若不填，由 GATE-P 确认后自动锁定）
- 团队规模：{e.g. 5人}
- 目标平台：{e.g. PC Steam}（若不填，由 GATE-P 提示用户选择）

⚠️ v2.0 新功能说明：
- 管线将在平台确认后，自动检测开发环境并引导 API Key 配置（GATE-S）
- 随后进行 AIGC 成本估算并提供三档方案（GATE-C，P0 级，不可跳过确认）
- 最终交付可运行游戏工程（含代码+资产），而非仅设计文档
```

**仅生成设计文档（快速模式，跳过 AIGC 和代码生成）：**
```
[系统] 你是游戏工业化 Master Agent v2.0。
请按 master-agent.md SOP 处理以下游戏想法，但在 GATE-S 环节回复「跳过」，
所有资产使用占位符，不执行 AIGC 生成和 DEV 编码阶段。

{用户游戏想法}
```

**单 Agent 直接执行：**
```
[系统] 你是 {Agent名称}，执行以下 Ticket。
强制遵循 protocol.md 中的 SSP v1.1 协议（Step 1 搜索优先）。

{TICKET 内容}
```

---

## 十一阶段 SOP 总览（v2.0 端到端生产管线）

```
阶段〇  环境准备层（v2.0 新增）
        ├── [GATE-P]  平台与技术栈确认门  ──→  引擎/包体/格式约束锁定
        ├── [GATE-S]  环境配置与API引导门 ──→  开发环境检测 + API Key 获取指引  ← v2.0 新增
        └── [GATE-C]  成本估算与路由选择门 ─→  AIGC 方案(A/B/C)确定 + 成本预警配置  ← v2.0 新增 [P0]
  ↓
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

| 接口 | 说明 | 新增版本 |
|------|------|---------|
| `Web_Search(query, domain_filter)` | 定向搜索，支持限定 GitHub/Reddit/Steam/GDC 等 | v1.0 |
| `Scrape_Comments(url)` | 提取指定页面用户评论，识别痛点与爽点 | v1.0 |
| `Fetch_OpenSource_Framework(repo_url)` | 从 GitHub 提取仓库架构摘要 | v1.0 |
| `Run_Shell_Command(command, purpose)` | 执行 Shell 命令（用于环境检测、工具链验证） | **v2.0** |
| `Call_AIGC_API(type, prompt, platform, params)` | 调用 AIGC 平台 API 生成图像/音频资产 | **v2.0** |
| `Write_File(path, mode, content)` | 写入文件（代码/资产归档/日志写盘） | **v2.0** |
| `Create_Directory(path)` | 创建目录（工程目录搭建） | **v2.0** |
| `List_Directory(path)` | 递归列出目录内容（交付清单生成） | **v2.0** |
| `Convert_Audio(input_path, output_path, target_format)` | 音频格式转换（平台适配，如 OGG→MP3） | **v2.0** |

---

## 文件索引

| 文件 | 内容 |
|------|------|
| [master-agent.md](master-agent.md) | Master Agent 完整 System Prompt（十一阶段 SOP v2.0 + GATE-P/S/C/0-7 全链路门控 + 12个固定层Agent动态派生 + 成本估算门 + 环境配置门 + AIGC 资产执行 + DEV 研发主程 + 死循环防护） |
| [sub-agents.md](sub-agents.md) | 固定层 M 个 + 动态层全品类 Sub-Agent System Prompts（含 DEV 研发主程 Agent） |
| [protocol.md](protocol.md) | SSP v1.1 独立协议文档（三步闭环 + AIGC 资产生成执行协议 + 代码编写执行协议） |
| [reference.md](reference.md) | 通用游戏设计速查手册（品类惯例 + 数值参考） |

### 运行时日志目录（自动生成）

| 路径 | 生成时机 | 内容 |
|------|---------|------|
| `_pipeline_log/00_project_context.md` | GATE-P/S/C/0 通过后 | 平台锁定 + 环境配置 + 成本方案 + 立项书 |
| `_pipeline_log/01_market_research.md` | 阶段二完成 | 市场调研与竞品报告 |
| `_pipeline_log/02_tickets.md` | 每发 Ticket 追加 | 所有 Ticket 工单 |
| `_pipeline_log/03_kanban.md` | 状态变更时覆盖 | 任务看板 |
| `_pipeline_log/04_agent_outputs/` | Sub-Agent 提交时 | 各 Agent 交付包 |
| `_pipeline_log/05_gate_log.md` | 每个 GATE 触发 | 用户决策记录 |
| `_pipeline_log/06_risk_register.md` | 阶段十一生成 | 风险登记簿 |
| `_pipeline_log/07_session_anchor.md` | 每次 GATE 后覆盖 | 会话恢复锚点 |
| `_pipeline_log/08_cost_routing.md` | GATE-C 通过后 | **v2.0 新增** 成本路由配置 + 实际支出追踪 |
| `_workspace/assets/` | AIGC 执行阶段 | 图像/音频/特效资产 |
| `_workspace/src/` | DEV Agent 执行 | 游戏工程源代码 |
| `_workspace/build_guide.md` | GATE-6 确认后 | 本地运行与编译指南 |
| `_workspace/.env` | GATE-S 配置后 | API Key 安全存储（不得提交 Git） |

