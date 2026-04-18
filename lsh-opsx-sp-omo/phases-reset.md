# Reset 阶段

重置变更，删除变更目录并重新开始。

---

## reset:<name>

```
1. 检查是否有未 commit 的变更
   - git status
   - 如有未暂存变更，提醒用户

2. 检查是否有 worktree
   - git worktree list
   - 如有该变更的 worktree，先清理

3. 删除变更目录
   - rm -rf openspec/changes/<name>/

4. 重新开始 propose 阶段
```
