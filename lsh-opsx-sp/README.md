# lsh-opsx-sp

## English

OpenSpec + SuperPowers workflow for TDD-first development.

### What This Is

A complete 3-phase development workflow combining:
- **OpenSpec**: Requirements documentation skeleton, change management, artifact templates
- **SuperPowers**: TDD execution, verification, code review, git worktree integration

### Core Principle

**Iron Law**: NO production code without TDD test first

### Phases

```
Phase 1: explore    → Brainstorming with lightweight requirements clarification
Phase 2: propose   → Delta Specs + design.md + tasks.md generation
Phase 3: apply     → TDD execution in isolated Git worktree
```

### Commands

```
/lsh-opsx-sp <topic>             # Full flow: explore → propose → apply
/lsh-opsx-sp explore:<topic>    # Start from explore phase
/lsh-opsx-sp propose:<change>   # Start from propose phase
/lsh-opsx-sp apply:<change>     # Apply changes (TDD in worktree, supports resume)
/lsh-opsx-sp reset:<change>     # Reset a change
/lsh-opsx-sp archive:<change>   # Archive completed changes
/lsh-opsx-sp continue:<change>  # Resume interrupted apply
```

### Auto Setup

On first use, the skill automatically:
1. Checks/installs openspec CLI
2. Initializes openspec if needed
3. Initializes git repo if needed

### When to Use

| Scenario | Use |
|----------|-----|
| New feature development | ✅ Yes |
| Bug fix with TDD | ✅ Yes |
| Simple changes | ✅ Yes |
| Complex parallel work | ❌ Consider lsh-opsx-sp-omo |

### Prerequisites

```bash
npm install -g opencode@latest
npm install -g @fission-ai/openspec@latest
openspec init --tools opencode
git init  # if not already a git repo
```

### Required Skills

- brainstorming
- test-driven-development
- verification-before-completion
- finishing-a-development-branch
- using-git-worktrees

### Gate Mechanism

Each phase transition requires explicit user confirmation:
- Gate-1: explore → propose
- Gate-2: design → tasks
- Gate-3: tasks → apply
- Gate-4: finishing → archive

---

## 中文

OpenSpec + SuperPowers 工作流，TDD 先行的开发流程。

### 核心原则

**铁律**：没有 TDD 测试，绝不写生产代码

### 三阶段流程

```
阶段 1: explore    → 头脑风暴 + 需求澄清
阶段 2: propose   → Delta Specs + design.md + tasks.md
阶段 3: apply     → 在隔离的 Git worktree 中执行 TDD
```

### 适用场景

| 场景 | 是否适用 |
|------|----------|
| 新功能开发 | ✅ 是 |
| Bug 修复 (TDD) | ✅ 是 |
| 简单变更 | ✅ 是 |
| 复杂并行工作 | ❌ 考虑 lsh-opsx-sp-omo |

### 自动前置检测

首次使用时，skill 会自动：
1. 检测/安装 openspec CLI
2. 初始化 openspec（如未初始化）
3. 初始化 git 仓库（如未初始化）

### 安装依赖

```bash
npm install -g opencode@latest
npm install -g @fission-ai/openspec@latest
openspec init --tools opencode
git init  # if not already a git repo
```

### 需要的 Skills

- brainstorming
- test-driven-development
- verification-before-completion
- finishing-a-development-branch
- using-git-worktrees

### 安装

Copy this folder to your OpenCode skills directory:
```
~/.config/opencode/skill/lsh-opsx-sp/
```

### 与 lsh-opsx-sp-omo 的区别

| Feature | lsh-opsx-sp | lsh-opsx-sp-omo |
|---------|-------------|-----------------|
| OMO Agents | ❌ No | ✅ Yes (Metis, Momus, Oracle, Hephaestus) |
| Parallel Execution | ❌ No | ✅ Hephaestus x5 |
| Complexity | Lower | Higher |
| Best for | Standard work | Complex changes |

---

## 📜 License

MIT License
