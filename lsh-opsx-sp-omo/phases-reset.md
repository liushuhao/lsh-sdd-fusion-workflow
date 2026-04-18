# Reset 阶段

重置变更，删除变更目录或 worktree 并重新开始。

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

## reset --force:<name>  ⚠️ 强制重置

```
1. 检查 worktree 状态
   - git worktree list
   - 强制删除 worktree（无论是否有未提交变更）

2. 强制删除变更目录
   - rm -rf openspec/changes/<name>/

3. 提示用户重新开始
   - "变更已重置，运行 /lsh-opsx-sp-omo apply:<name> 重新开始"
```

## apply 中的"重新开始"选项

在 apply 状态检测时，如用户选择"重新开始"：
```
1. 询问确认
   - "确定要删除现有 worktree 并重新开始吗？所有未提交的更改将丢失。"
   - "确认" → 执行 reset --force
   - "取消" → 返回状态检测

2. 执行 reset --force:<name>

3. 重新创建 worktree 并开始执行
```
