# Apply 阶段

在隔离环境中实现变更。通过 OMO Hephaestus x5 并行执行、TDD、code review，双重验证保证质量。

---

## 目标

在隔离环境中实现变更。**通过 OMO Hephaestus x5 并行执行、TDD、code review，双重验证保证质量。**

## OMO 并行执行节点

| 节点 | 智能体 | 目的 |
|------|--------|------|
| 任务编排 | 分析 tasks.md 依赖 | 识别可并行任务组 |
| 并行执行 | `task(subagent_type: "hephaestus")` x5 | TDD 循环并行执行 |

## 流程

```
1. openspec status --change <name> --json
   openspec instructions apply --change <name> --json

2. 【Gate-check】Artifacts 完整性检查
   openspec verify --change <name>
   - IF 任何 artifact 缺失 → 拒绝执行
   - ELSE 继续

3. 【状态检测】⚠️ 断点续传支持
   检查是否存在未完成的 worktree：
   - IF worktree 存在 → 检测 tasks.md 进度
     - 显示："检测到未完成变更，继续？"
     - 用户选择"继续" → 从断点执行
     - 用户选择"跳过" → 跳过阻塞任务
     - 用户选择"重新开始" → 删除旧 worktree，重新创建
   - ELSE worktree 不存在 → 继续步骤 5

5. 【读取】初始上下文
   - openspec/specs/
   - openspec/changes/<name>/design.md
   - openspec/changes/<name>/tasks.md

6. skill("using-git-worktrees")
   - 创建 git worktree 隔离环境

7. 【分析任务依赖图】⚠️ OMO 任务编排
   分析 tasks.md 中的任务依赖关系，识别可并行执行的任务组：
   - 无依赖任务 → 可并行
   - 有依赖任务 → 按依赖顺序执行
   
   输出：任务分组列表

8. 【Hephaestus x5 并行执行】⚠️ OMO 增强
   5 个 Hephaestus 智能体并行执行 TDD 循环：
   
   task(subagent_type: "hephaestus") x5
   - 每个 Hephaestus 执行独立任务
   - 使用 skill("test-driven-development") 执行 TDD 循环
   - 完成后调用 skill("requesting-code-review") 审查
   
   **对每个任务执行：**
   
   skill("test-driven-development") ⚠️ Iron Law: NO PRODUCTION CODE WITHOUT FAILING TEST FIRST
   
   **Red-Green-Refactor 循环：**
   - RED: 写失败的测试
   - VERIFY RED: 确认测试失败原因正确
   - GREEN: 写最小代码通过测试
   - VERIFY GREEN: 确认测试通过
   - REFACTOR: 清理代码
   
   **每任务完成后：**
   - skill("requesting-code-review")
   - 标记任务完成 - [ ] → - [x]

9. 如 TDD 测试失败 → skill("systematic-debugging") ⚠️ OMO Oracle 协助
   - 追溯根因
   - 如需架构决策，调用 task(subagent_type: "oracle")
   - 读取 skill(systematic-debugging)/root-cause-tracing.md
   - 读取 skill(systematic-debugging)/condition-based-waiting.md
   - 读取 skill(systematic-debugging)/defense-in-depth.md

10. 双重验证 ⚠️ OpenSpec + SuperPowers
    - OpenSpec 维度：
      openspec verify --change <name>
      检查 Completeness / Correctness / Coherence
    
    - SuperPowers 维度：
      skill("verification-before-completion")
      验证代码质量、测试覆盖率
    
    - 如验证失败 → 触发 Fluid 回退

11. skill("finishing-a-development-branch")
    - 验证测试通过
    - Git commit + tag
    - 呈现合并选项
    - 执行用户选择的合并操作

12. skill("openspec-archive-change")
    - 检查 artifact 完成状态
    - 检查 task 完成状态
    - 评估 delta spec 同步状态
    - 执行 archive（移动到 openspec/changes/archive/）

13. 完成
```
