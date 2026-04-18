# Knowledge - 知识管理

本文档描述 OpenSpec + SuperPowers 融合工作流的知识管理机制。

---

## OpenSpec 设计知识

OpenSpec 通过文件系统和 Delta Specs 机制实现知识积累：

### 1. Delta Specs 合并

每次 archive 时，delta specs 合并到主 specs：
```
变更的 specs/<domain>/spec.md
    ↓ 合并
openspec/specs/<domain>/spec.md
```

这使得系统规格随时间累积。

### 2. Archive 历史

归档的变更保留了完整的设计决策历史：
```
openspec/changes/archive/YYYY-MM-DD-<name>/
├── proposal.md        # 当时的意图
├── design.md          # 当时的设计
├── tasks.md           # 当时的任务
└── specs/            # 当时的 delta specs
```

---

## 知识复用循环

```
┌─────────────────────────────────────────────────────────────┐
│                    知识复用循环                              │
│                                                             │
│  1. propose 开始 → 读取 openspec/specs/                  │
│                    ↓                                         │
│  2. 读取相关 design.md（避免重复设计）                   │
│                    ↓                                         │
│  3. archive → delta specs 合并 → specs/ 更新            │
│                    ↓                                         │
│  4. 下次变更 → 读取最新 specs → 复用                    │
└─────────────────────────────────────────────────────────────┘
```

---

## SuperPowers 方法论（不是知识积累）

SuperPowers Skills 提供方法论指导，不提供知识积累存储：

| Skill | 提供 |
|-------|------|
| brainstorming | 需求澄清方法 |
| writing-plans | 任务规划方法 |
| test-driven-development | TDD 流程 |
| systematic-debugging | 调试方法 |
| verification-before-completion | 验证方法 |

---

## 不要做的事情

❌ **不要**在 `openspec/rules/` 下创建任何文件（OpenSpec 不认识这个目录）

❌ **不要**在 OpenSpec 目录下存储 SuperPowers 的知识积累

✅ **应该**通过 delta specs 合并让 OpenSpec specs/ 目录自然增长
