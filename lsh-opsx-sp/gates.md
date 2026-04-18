# Gates - Gate 机制与完成标准

本文档包含 Gate 机制、Fluid 回退机制和完成标准。

---

## Gate 机制

### Gate 概览

每个 phase 之间的转换必须满足明确的条件（Gate），AI 不得自行跳过。

| Gate | Phase 转换 | Gate 条件 | 触发方式 |
|------|-----------|----------|---------|
| Gate-1 | explore → propose | 用户明确授权 | AI 输出"需求澄清摘要"，用户回复"继续"或"开始提案" |
| Gate-2 | design → tasks | 用户确认设计方案 | 用户回复"同意"或"需要修改" |
| Gate-3 | tasks → apply | 用户确认任务清单 | 用户回复"开始实现"或"需要调整" |
| Gate-4 | finishing + archive → 完成 | finishing 选项已执行 + archive 完成 | 用户确认归档 |

### Gate 执行规则

1. **显式确认优先**：每个 Gate 必须输出明确的问题或选项，等待用户回复
2. **沉默不是同意**：用户无回应时，AI 应提示"请确认是否继续"
3. **AI 不得自行判断跳过**：即使 AI 认为"需求已清晰"，也必须经过用户确认才能进入下一 phase
4. **缺失 artifacts 则拒绝 apply**：如果 design.md 或 tasks.md 不存在，必须先完成 propose 阶段

### Gate 自动呈现格式

到达 Gate 时自动呈现状态摘要 + 推荐选项：

| Gate | 位置 | 自动呈现内容 | 推荐选项 |
|------|------|-------------|---------|
| Gate-1 | explore 结束后 | 需求澄清摘要 + 确认意图 | [推荐] 继续 / 开始提案 / 取消 |
| Gate-2 | design.md 生成后 | 设计方案摘要 + Delta Specs 预览 + 风险评估 | [推荐] 同意 / 需要修改 / 取消 |
| Gate-3 | tasks.md 生成后 | verify 结果 + 任务清单 + 预计工时 + TDD 步骤预览 | [推荐] 开始实现 / 需要调整 / 取消 |
| Gate-4 | finishing + archive 结束后 | Git 操作完成 + archive 完成 | finishing 选项 + archive 确认 |

---

## 验证失败处理

### 双重验证执行节点

在 apply 阶段末尾执行双重验证：

```
双重验证
   ├── OpenSpec 维度验证
   │   ├── Completeness（完整性）：specs 是否覆盖所有需求
   │   ├── Correctness（正确性）：实现是否符合 spec
   │   └── Coherence（一致性）：specs 内部是否一致
   │
   └── SuperPowers 维度验证
       └── skill("verification-before-completion") ⚠️ Iron Law: NO CLAIMS WITHOUT EVIDENCE
           └── 验证代码质量、测试覆盖率、文档同步
```

### 验证失败场景

| 验证失败场景 | AI 处理方式 |
|-------------|-------------|
| OpenSpec 验证失败 | 输出验证失败原因，等待用户决策 |
| SuperPowers 验证失败 | 最多重试 3 次 |
| 双重验证均失败 | 进入 Fluid 回退流程 |

### 双重验证失败处理流程

```
双重验证执行
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│  OpenSpec 维度验证                                        │
│  openspec verify --change <name>                          │
│  IF Completeness / Correctness / Coherence 任意失败       │
│    → 输出具体失败原因                                      │
│    → 等待用户决策：修复 / 取消                          │
└─────────────────────────────────────────────────────────────┘
    │ 通过
    ▼
┌─────────────────────────────────────────────────────────────┐
│  SuperPowers 维度验证                                     │
│  skill("verification-before-completion")                  │
│    → 最多重试 3 次                                       │
│    → 3 次仍失败 → 触发 Fluid 回退                        │
└─────────────────────────────────────────────────────────────┘
    │ 通过
    ▼
  继续 finishing 流程
```

---

## Fluid 回退机制

Fluid 回退是指在实现过程中发现问题后，根据问题性质选择不同级别的回退。

| 问题类型 | 回退级别 | 触发条件 | 处理方式 |
|---------|---------|---------|---------|
| 单任务问题 | 任务级回退 | 单个任务 TDD 失败 | 定位问题根因，修复后继续该任务 |
| 多任务问题 | 阶段级回退 | 多个任务有依赖性问题 | 回退到 propose 阶段，更新 design.md 或 tasks.md |
| 全局性问题 | 架构级回退 | 验证发现架构设计缺陷 | 回退到 explore 阶段，重新探索需求 |

### Fluid 回退执行流程

```
1. 识别问题影响范围
2. 确定回退级别（任务/阶段/架构）
3. 【Gate-check】向用户说明回退原因和影响
4. 更新相关 artifacts
5. 用户确认回退后的 artifacts
6. 根据回退级别重新执行对应阶段
7. 继续原流程
```

---

## 完成标准

每次变更（无论大小）完成后，必须逐项确认：

1. **代码变更**：已提交到 Git（git commit）
2. **归档**：变更记录已归档到 `openspec/changes/archive/`（Gate-4 确认后）
3. **Delta Specs 已合并**：delta specs 已同步到主 specs

这三个步骤必须全部完成才能标记变更结束。
