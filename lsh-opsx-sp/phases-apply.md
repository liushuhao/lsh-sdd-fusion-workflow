# Apply 阶段

在隔离环境中实现变更。通过 TDD、code review、双重验证保证质量。支持断点续传。

---

## 目标

在隔离环境中实现变更。通过 TDD、code review，双重验证保证质量。**支持中断后继续执行**。

## 流程

```
1. openspec status --change <name> --json
   openspec instructions apply --change <name> --json

2. 【Gate-check】Artifacts 完整性检查
   openspec verify --change <name>
   - IF 任何 artifact 缺失 → 拒绝执行
   - ELSE 继续

3. 【状态检测】⚠️ 新增：断点续传支持
   检查是否存在未完成的 worktree：
   - IF worktree 存在 → 检测 tasks.md 进度
     - 显示："检测到未完成变更，继续？"
     - 用户选择"继续" → 从断点执行
     - 用户选择"跳过" → 跳过阻塞任务
     - 用户选择"重新开始" → 删除旧 worktree，重新创建
   - ELSE worktree 不存在 → 继续步骤 4

4. 【读取】初始上下文
   - openspec/specs/
   - openspec/changes/<name>/design.md
   - openspec/changes/<name>/tasks.md

5. skill("using-git-worktrees")
   - 检查 worktree 是否已存在
   - 存在则复用，不存在则创建

6. skill("subagent-driven-development")
   - 按 tasks.md 顺序执行任务
   - 每个任务经过 TDD 循环
   - 任务间有审查 checkpoints

7. 【TDD 循环】⚠️ 融合关键点 - SuperPowers TDD 执行
   对每个任务执行：
   
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

8. 【阻塞处理】⚠️ 新增：网络/权限等不可抗力处理
   如遇到阻塞（网络超时、权限问题等）：
   - 立即保存当前进度到 git
   - 询问用户：
     - "继续等待" → 保持状态，等待用户指令
     - "跳过此任务" → 标记跳过 - [s]，继续下一个
     - "取消" → 退出，状态已保存，可下次 continue
   - 保存阻塞原因到 worktree 根目录 `.blocker-info`

9. 如 TDD 测试失败 → skill("systematic-debugging") ⚠️ Iron Law: NO FIXES WITHOUT ROOT CAUSE
   - 读取 root-cause-tracing.md
   - 读取 condition-based-waiting.md
   - 读取 defense-in-depth.md
   - 追溯根因

10. 【进度保存】⚠️ 新增：定期保存进度
    每完成 3 个 task 或遇到阻塞时：
    - 自动 git commit 当前进度
    - commit message 格式："progress: completed N/M tasks"
    - 遇到阻塞立即保存状态

11. 双重验证 ⚠️ OpenSpec + SuperPowers
    - OpenSpec 维度：
      openspec verify --change <name>
      检查 Completeness / Correctness / Coherence
    
    - SuperPowers 维度：
      skill("verification-before-completion")
      验证代码质量、测试覆盖率
    
    - 如验证失败 → 触发 Fluid 回退

12. skill("finishing-a-development-branch")
    - 验证测试通过
    - Git commit + tag
    - 呈现合并选项
    - 执行用户选择的合并操作

13. skill("openspec-archive-change")
    - 检查 artifact 完成状态
    - 检查 task 完成状态
    - 评估 delta spec 同步状态
    - 执行 archive（移动到 openspec/changes/archive/）

14. 完成
```
