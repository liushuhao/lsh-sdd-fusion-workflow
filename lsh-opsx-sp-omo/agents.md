# OMO Agents - 智能体调用参考

本文档记录 oh-my-opencode 智能体的调用方式，用于 lsh-opsx-sp-omo 工作流。

---

## 可调用的智能体

### 顾问型智能体

| 智能体 | 调用方式 | 用途 | 温度 |
|--------|---------|------|------|
| **Metis** | `task(subagent_type: "metis")` | design.md 隐藏问题分析 | 0.3 |
| **Momus** | `task(subagent_type: "momus")` | tasks.md 计划审查 | 0.1 |
| **Oracle** | `task(subagent_type: "oracle")` | 架构决策/调试（按需） | 0.1 |
| **Librarian** | `task(subagent_type: "librarian")` | 文档/代码搜索 | 0.1 |
| **Explore** | `task(subagent_type: "explore")` | 快速代码探索 | 0.1 |

### 执行型智能体

| 智能体 | 调用方式 | 用途 | 温度 |
|--------|---------|------|------|
| **Hephaestus** | `task(subagent_type: "hephaestus")` | TDD 循环并行执行 | 0.1 |

**并行数量**：5（`task(Hephaestus) x5`）

---

## 不可调用的智能体

| 智能体 | Mode | 原因 | 替代方案 |
|--------|------|------|---------|
| **Prometheus** | internal | 仅供 Sisyphus 内部使用 | Metis（pre-planning） |
| **Atlas** | primary | 只能在 UI 选择 | 手动任务编排 |
| **Sisyphus** | primary | 主编排器 | skill(finishing-a-development-branch) |

---

## 智能体调用示例

### Metis - 隐藏问题分析

```typescript
task({
  subagent_type: "metis",
  prompt: `分析以下 design.md，识别隐藏意图和潜在问题：
  
  [读取 design.md 内容]
  
  输出：
  1. 遗漏的问题
  2. 歧义点
  3. AI 失败点分析`
})
```

### Momus - 计划审查

```typescript
task({
  subagent_type: "momus",
  prompt: `审查以下 tasks.md 计划：
  
  [读取 tasks.md 内容]
  
  验证：
  1. 清晰度 - 任务描述是否明确
  2. 可测量性 - 成功标准是否可衡量
  3. 上下文充分性 - 是否有足够上下文执行`
})
```

### Hephaestus x5 - 并行执行

```typescript
// 5 个 Hephaestus 并行执行 TDD 循环
const hephaestusTasks = tasks.map(task => 
  task({
    subagent_type: "hephaestus",
    prompt: `执行 TDD 循环：
    
    任务：${task.description}
    
    步骤：
    1. 写测试（RED）
    2. 写实现（GREEN）
    3. 重构（REFACTOR）
    4. 验证测试通过
    
    使用 skill('test-driven-development')`
  })
)
```

### Oracle - 架构决策

```typescript
task({
  subagent_type: "oracle",
  prompt: `分析以下架构决策问题：
  
  [描述问题]
  
  提供：
  1. 技术方案选项
  2. 利弊分析
  3. 风险评估
  4. 推荐方案`
})
```

---

## 工具限制

| 智能体 | 禁止使用的工具 |
|--------|--------------|
| Oracle | write, edit, task, call_omo_agent |
| Librarian | write, edit, task, call_omo_agent |
| Explore | write, edit, task, call_omo_agent |
| Momus | write, edit, task |
| Atlas | task, call_omo_agent |
| Hephaestus | task, delegate_task, call_omo_agent（防止递归） |

---

## 来源

本文件参考 [oh-my-opencode AGENTS.md](https://github.com/code-yeongyu/oh-my-opencode/blob/dev/src/agents/AGENTS.md)，版本：dev branch，2026-04-11。
