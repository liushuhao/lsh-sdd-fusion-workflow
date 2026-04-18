# lsh-opsx-sp-omo

## English

OpenSpec + SuperPowers + OMO workflow — enhanced with AI agents for complex development.

### What This Is

An enhanced 3-phase development workflow combining:
- **OpenSpec**: Requirements documentation skeleton, change management, artifact templates
- **SuperPowers**: TDD execution, verification, code review, git worktree integration
- **OMO Agents**: Metis, Momus, Oracle, Hephaestus x5

### Core Principle

**Iron Law**: NO production code without TDD test first

### OMO Agents

| Agent | Role | Usage |
|-------|------|-------|
| **Metis** | Pre-planning consultant | Hidden problem analysis |
| **Momus** | Plan reviewer | Quality verification |
| **Oracle** | Architecture consultant | Debugging, architecture decisions |
| **Hephaestus** | Parallel executor | x5 TDD loops in parallel |

### Phases

```
Phase 1: explore    → Brainstorming (SuperPowers, lightweight)
Phase 2: propose   → Delta Specs + Metis hidden problem analysis + Momus review
Phase 3: apply     → Hephaestus x5 parallel TDD execution
```

### Commands

```
/lsh-opsx-sp-omo <topic>            # Full flow with OMO enhancement
/lsh-opsx-sp-omo explore:<topic>   # Start from explore phase
/lsh-opsx-sp-omo propose:<change>  # Start from propose phase
/lsh-opsx-sp-omo apply:<change>    # Hephaestus x5 parallel TDD
/lsh-opsx-sp-omo reset:<change>    # Reset a change
/lsh-opsx-sp-omo archive:<change>  # Archive completed changes
/lsh-opsx-sp-omo continue:<change> # Resume interrupted apply
```

### Auto Setup

On first use, the skill automatically:
1. Checks/installs openspec CLI
2. Initializes openspec if needed
3. Initializes git repo if needed

### When to Use

| Scenario | Use |
|----------|-----|
| Complex multi-task changes | ✅ Yes |
| Parallel TDD execution needed | ✅ Yes |
| Hidden problem analysis | ✅ Metis |
| Architecture decisions | ✅ Oracle |
| Standard development | ⚠️ Consider lsh-opsx-sp |

### Prerequisites

```bash
npm install -g opencode@latest
npm install -g @fission-ai/openspec@latest
openspec init --tools opencode
git init  # if not already a git repo
```

### Required Skills

All SuperPowers skills:
- brainstorming
- test-driven-development
- verification-before-completion
- finishing-a-development-branch
- using-git-worktrees

Plus OMO agents:
- Metis
- Momus
- Oracle
- Hephaestus (x5)

> ⚠️ **Important**: This workflow uses `openspec/changes/<name>/tasks.md` for task output, but the `writing-plans` skill defaults to `docs/superpowers/plans/`. To align the output path, modify your `writing-plans` skill:
> - Edit: `~/.config/opencode/skill/writing-plans/SKILL.md`
> - Change line 20 from: `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
> - To: `openspec/changes/<name>/tasks.md`

### Gate Mechanism

Each phase transition requires explicit user confirmation:
- Gate-1: explore → propose
- Gate-2: design → tasks (Momus review)
- Gate-3: tasks → apply
- Gate-4: finishing → archive

---

## 中文

OpenSpec + SuperPowers + OMO 工作流 — 融合 AI 智能体，适用于复杂开发场景。

### 核心原则

**铁律**：没有 TDD 测试，绝不写生产代码

### OMO 智能体

| 智能体 | 角色 | 用途 |
|--------|------|------|
| **Metis** | 预规划顾问 | 隐藏问题分析 |
| **Momus** | 计划审查员 | 质量验证 |
| **Oracle** | 架构顾问 | 调试、架构决策 |
| **Hephaestus** | 并行执行器 | x5 TDD 循环并行执行 |

### 三阶段流程

```
阶段 1: explore    → 头脑风暴 (SuperPowers，轻量模式)
阶段 2: propose   → Delta Specs + Metis 隐藏问题分析 + Momus 计划审查
阶段 3: apply     → Hephaestus x5 并行 TDD 执行
```

### 适用场景

| 场景 | 是否适用 |
|------|----------|
| 复杂多任务变更 | ✅ 是 |
| 需要并行 TDD | ✅ 是 |
| 隐藏问题分析 | ✅ Metis |
| 架构决策 | ✅ Oracle |
| 标准开发 | ⚠️ 考虑 lsh-opsx-sp |

### 安装依赖

```bash
npm install -g opencode@latest
npm install -g @fission-ai/openspec@latest
openspec init --tools opencode
git init  # if not already a git repo
```

### 自动前置检测

首次使用时，skill 会自动：
1. 检测/安装 openspec CLI
2. 初始化 openspec（如未初始化）
3. 初始化 git 仓库（如未初始化）

### 需要的 Skills

所有 SuperPowers skills：
- brainstorming
- test-driven-development
- verification-before-completion
- finishing-a-development-branch
- using-git-worktrees

加上 OMO 智能体：
- Metis
- Momus
- Oracle
- Hephaestus (x5)

> ⚠️ **重要**：此工作流使用 `openspec/changes/<name>/tasks.md` 作为任务输出路径，但 `writing-plans` skill 默认输出到 `docs/superpowers/plans/`。如需对齐输出路径，请修改 `writing-plans` skill：
> - 编辑：`~/.config/opencode/skill/writing-plans/SKILL.md`
> - 将第 20 行从：`docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
> - 改为：`openspec/changes/<name>/tasks.md`

### 安装

Copy this folder to your OpenCode skills directory:
```
~/.config/opencode/skill/lsh-opsx-sp-omo/
```

### 与 lsh-opsx-sp 的区别

| Feature | lsh-opsx-sp | lsh-opsx-sp-omo |
|---------|-------------|-----------------|
| OMO Agents | ❌ No | ✅ Yes (Metis, Momus, Oracle, Hephaestus) |
| Parallel Execution | ❌ No | ✅ Hephaestus x5 |
| Complexity | Lower | Higher |
| Best for | Standard work | Complex changes |

---

## 📜 License

MIT License
