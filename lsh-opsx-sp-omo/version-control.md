# Version Control - 版本控制

本文档描述 Git 版本控制和 OpenSpec Archive 流程。

---

## Git 规范

### 分支命名

```
<type>/<change-name>

Examples:
feature/add-dark-mode
fix/login-performance
refactor/cleanup-db
```

### 分支类型

| 类型 | 用途 | 合并目标 |
|------|------|---------|
| feature | 新功能 | main |
| fix | bug 修复 | main |
| refactor | 重构 | main |
| docs | 文档 | main |
| test | 测试 | main |
| chore | 杂项 | main |

### Commit 规范

格式：
```
<type>(<scope>): <subject>

<body>

<footer>
```

类型：

| 类型 | 说明 |
|------|------|
| feat | 新功能 |
| fix | bug 修复 |
| docs | 文档 |
| style | 格式（不影响代码）|
| refactor | 重构 |
| test | 测试 |
| chore | 杂项 |

示例：
```
feat(auth): add remember me functionality

- Add cookie-based session persistence
- Add preference detection
- Update login flow

Closes #123
```

### Tag 规范

```
<change-name>

Examples:
add-dark-mode-2025-01-24
optimize-query-2025-01-25
```

---

## 版本控制概览

OpenSpec 融合工作流的版本控制由 `finishing-a-development-branch` skill 处理 Git 操作（commit、merge、worktree 清理），OpenSpec archive 由 AI 在 Gate-4 显式执行。

---

## Git 操作流程

```
finishing-a-development-branch（Git 操作中心）
├── Step 1: 验证测试通过
├── Step 2: 创建变更快照 commit
│   - git add openspec/changes/<name>/
│   - git commit -m "feat: complete <name>"
│   - git tag <name>-<date>
├── Step 3: 呈现合并选项
├── Step 4: 执行所选操作（merge/PR/keep）
└── Step 5: 清理 worktree
    - git worktree remove <branch>

Gate-4 → OpenSpec Archive
├── skill("openspec-archive-change") 调用
├── 合并 delta specs → 主 specs
└── 移动变更目录到 archive/
```

---

## Git 操作详解

### Step 2: 创建变更快照 Commit

```bash
git add openspec/changes/<name>/
git commit -m "feat: complete <name>"
git tag <name>-<date>
```

### Step 4: 合并选项

| 选项 | 操作 | 适用场景 |
|------|------|---------|
| merge to main | `git merge` | 简单变更，直接合并 |
| create PR | 创建 Pull Request | 需要审查的变更 |
| keep branch | 保持 feature 分支 | 后续继续开发 |

### Step 5: 清理 Worktree

```bash
git worktree remove <branch>
git branch -d <branch>
```

---

## OpenSpec Archive ⚠️ 路径修正

### Archive 流程

**实际路径**：`openspec/changes/archive/YYYY-MM-DD-<name>/`（CLI 行为）

```
Before archive:
openspec/
├── specs/ ◄──┐ merge
└── changes/<name>/specs/ ──┘

After archive:
openspec/
├── specs/                              # 包含变更需求（delta 已合并）
└── changes/archive/
    └── YYYY-MM-DD-<name>/              # 实际路径
```

### Archive 操作

skill("openspec-archive-change") 调用后执行：
1. 合并 delta specs 到主 specs
2. 移动变更目录到 `openspec/changes/archive/YYYY-MM-DD-<name>/`

### Archive 命令

```bash
# 方式 1: 使用 skill
skill("openspec-archive-change")

# 方式 2: 直接使用 CLI
openspec archive <name>
```

---

## 版本控制优势

| 优势 | 说明 |
|------|------|
| **版本完整** | 代码 + artifacts 同一 commit |
| **便于追溯** | Git log 可看到完整变更历史 |
| **Tag 标记** | 通过 tag 快速定位版本 |
| **自动清理** | worktree 分支自动管理 |
| **规格同步** | delta specs 自动合并 |

---

## 检查清单

完成版本控制流程后，确认：

**finishing-a-development-branch（Git 操作）：**
- [ ] Git commit 已创建
- [ ] Git tag 已创建
- [ ] 合并/PR 已完成
- [ ] worktree 已清理

**OpenSpec Archive（Gate-4）：**
- [ ] skill("openspec-archive-change") 已调用
- [ ] delta specs 已合并到 openspec/specs/
- [ ] 变更目录已移动到 openspec/changes/archive/
