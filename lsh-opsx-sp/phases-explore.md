# Explore 阶段

探索想法、调查问题、澄清需求。轻量级 brainstorming。

---

## 目标

探索想法、调查问题、澄清需求。轻量级 brainstorming。

## 流程

```
1. 【读取】OpenSpec 上下文
   - openspec list --json  (检查现有变更)
   - openspec/specs/       (了解系统已有规格)
   - openspec/changes/archive/  (近期归档变更)

2. skill("brainstorming") 轻量模式 ⚠️ HARD-GATE
   - 探索项目上下文
   - 逐个提问澄清需求
   - 提出 2-3 个方案，明确说明利弊，让用户选择
   - 呈现设计思路

【Gate-1】输出"需求澄清摘要"，等待用户确认
   - "继续" → 继续探索
   - "开始提案" → 触发 propose 阶段
   - "取消" → 结束会话，保存探索笔记到 `openspec/changes/<name>/exploration-notes.md`
```

## 产出

Explore 不强制产出文档，但可以：
- 在对话中记录关键洞察
- 标注需要进一步探索的点
- 总结已明确的需求
