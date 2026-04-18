# Propose 阶段

创建完整的变更提案。使用 OpenSpec CLI 获取模板，SuperPowers brainstorming 填充内容。

---

## 目标

创建完整的变更提案。**使用 OpenSpec CLI 获取模板，SuperPowers brainstorming 填充内容。**

## 流程

```
1. openspec new change <name>

2. 【读取】相关 design.md（知识复用：避免重复设计）

3. 【获取 artifact 模板】⚠️ 融合关键点
   openspec instructions proposal --change <name> --json
   openspec instructions design --change <name> --json
   openspec instructions tasks --change <name> --json
   
   解析 JSON 获取：
   - template: 各 artifact 的结构模板
   - context: 项目背景约束
   - rules: artifact 特定规则
   - outputPath: 输出路径

4. skill("brainstorming") 完整模式
   - 探索项目上下文
   - 逐个提问澄清需求
   - 提出 2-3 个方案，明确说明利弊
   - 分段呈现设计，逐段确认
   - ⚠️ **重要**: brainstorming 只做需求澄清，**不生成任何文件**
   - 所有 artifact 由 lsh-opsx-sp 基于 brainstorming 结果生成

5. 【生成 proposal.md】
   基于 brainstorming 结果，生成变更提案：
   
   **输出路径**: `openspec/changes/<name>/proposal.md`
   
   ```markdown
   ## Intent
   
   ## Scope
   - In scope
   - Out of scope
   ```

6. 【生成 Delta Specs】⚠️ OpenSpec 特色
   基于 brainstorming 结果，生成 Given/When/Then 格式的 Delta Specs：
   
   **输出路径**: `openspec/changes/<name>/specs/<domain>/spec.md`
   
   ```markdown
   # specs/<domain>/spec.md
   ### Requirement: [功能名]
   The system SHALL [行为描述]
   
   #### Scenario: [场景名]
   Given: [前置条件]
   When: [触发动作]
   Then: [预期结果]
   ```

7. 【生成 design.md】⚠️ 使用 OpenSpec 模板
   按 openspec instructions 返回的 template 填充内容：
   
   **输出路径**: `openspec/changes/<name>/design.md`
   
   ```markdown
   ## Intent
   ## Technical Approach
   ## Architecture
   ## Data Flow
   ## Error Handling
   ```

9. 【spec self-review】⚠️ SuperPowers 特色
   调用 brainstorming skill 进行 spec self-review：
   - 检查 design.md：
   - Placeholder scan: 无 TBD/TODO
   - Internal consistency: 无矛盾
   - Scope check: 范围明确
   - Ambiguity check: 无歧义

10. 【Gate-2】呈现设计方案，等待用户确认
    - 展示 design.md 摘要 + Delta Specs 预览
    - "同意" → 继续
    - "需要修改" → 修改后重新 Gate-2

11. skill("writing-plans") ⚠️ SuperPowers TDD 任务
    基于 design.md 生成 tasks.md（**tasks.md 使用 SuperPowers 模板，不是 design.md**）：
    
    **⚠️ 重要**: writing-plans 只生成任务内容，**不写入文件**
    lsh-opsx-sp 将内容写入以下路径：
    **输出路径**: `openspec/changes/<name>/tasks.md`
    
    **tasks.md 格式要点：**
    - 使用 SuperPowers writing-plans 的 Task 结构
    - 保持 OpenSpec 的分层编号（如 `1.1`, `1.2`, `2.1`）
    - TDD 步骤：RED (写测试) → GREEN (写代码) → REFACTOR
    
    **详细模板见本文档「TDD 任务格式」章节。**

12. 【openspec verify】检查 artifacts 完整性
    openspec verify --change <name>

13. 【Gate-3】呈现任务清单，等待用户确认
    - 展示 tasks.md 预览
    - "开始实现" → 继续
    - "需要调整" → 修改后重新 Gate-3

14. 生成完成提示
    "所有 artifacts 已生成！运行 /lsh-opsx-sp apply:<name> 开始实现"
```

## artifact 产出

| Artifact | 输出路径 | 模板来源 | 内容来源 | 传入 Apply |
|----------|---------|---------|---------|-----------|
| `proposal.md` | `openspec/changes/<name>/proposal.md` | OpenSpec CLI | brainstorming 结果 | ❌ 仅作为记录 |
| `specs/<domain>/spec.md` | `openspec/changes/<name>/specs/<domain>/spec.md` | OpenSpec Delta Specs | Given/When/Then | ❌ 按需参考 |
| `design.md` | `openspec/changes/<name>/design.md` | OpenSpec CLI | brainstorming + 架构分析 | ✅ 传入 Apply |
| `tasks.md` | `openspec/changes/<name>/tasks.md` | **SuperPowers writing-plans** | TDD 五步 + OpenSpec 编号 | ✅ 唯一任务来源 |

---

## TDD 任务格式（SuperPowers 注入 OpenSpec tasks.md）

```markdown
## N. [组件名]

**Files:**
- Create: `src/components/Component.tsx`
- Modify: `src/styles/theme.css:1-50`

- [ ] **N.1 Write failing test**
  ```tsx
  test('Component renders with correct initial state', () => {
    render(<Component />);
    expect(screen.getByRole('button')).toBeInTheDocument();
  });
  ```
  Run: `npm test -- --testPathPattern="Component" --watchAll=false`
  Expected: FAIL - "Component is not defined"

- [ ] **N.2 Implement minimal code**
  ```tsx
  export function Component() {
    return <button>Click me</button>;
  }
  ```
  Run: `npm test -- --testPathPattern="Component" --watchAll=false`
  Expected: PASS

- [ ] **N.3 Refactor if needed**
  - Extract any duplication
  - Improve naming
  Run tests again to verify still green.

- [ ] **N.4 Commit**
  ```bash
  git add src/components/Component.tsx tests/Component.test.tsx
  git commit -m "feat: add Component [skip ci]"
  ```
```
