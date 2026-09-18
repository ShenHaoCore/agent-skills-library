---
name: time-blocking
description: >
  Guides time-blocking and calendar-based focus planning: themed blocks, buffers,
  deep-work sessions, batching, and realistic day/week schedules.
  USE FOR: protecting deep work on a busy calendar; planning tomorrow/this week;
  themed days or themed blocks for writing, coding, or learning; batching shallow work.
  关键词：时间块、time-blocking、日程、专注时段、calendar、深度工作、主题日。
  DO NOT USE FOR: deciding which tasks exist or GTD clarify (use task-management);
  multi-month roadmaps (use business/project-planning); PKM/note structure
  (use note-taking).
license: MIT
---

# Time Blocking

把重要任务放进日历上的具体时间块，用边界保护专注。

任务澄清与优先级先做 `productivity/task-management`；会议/读书知识沉淀用
`productivity/note-taking`；跨周月的里程碑排期用 `business/project-planning`。

## When to Use

- 日程被会议打散，需要深度工作块
- 规划明日 / 本周日历
- 为写作、编码、学习设置主题日或主题块
- 批处理邮件、审批等浅工作

## When Not to Use

- 尚不清楚下一步动作或 Inbox 未澄清 → `productivity/task-management`
- 多月路线图、里程碑依赖 → `business/project-planning`
- 设计笔记库结构 → `productivity/note-taking`
- 把所有待办直接丢进日历当垃圾桶（应先任务库，再选块）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 可编辑日历 | Yes | 个人或工作日历写权限 |
| 今日/本周 Top 1–3 | Yes | 来自任务系统的优先项 |
| 固定事件列表 | Recommended | 会议、接送等不可移动约束 |
| 精力偏好 | Optional | 上午/下午更适合深度工作 |

## Workflow

### Step 1: 先钉死固定事件

放入会议、接送等硬约束，再谈深度块。

### Step 2: 放置深度工作块

深度块 60–120 分钟；块间留缓冲 10–15 分钟。每天至少 1 个「不可移动」的最重要块。

### Step 3: 批处理浅工作

邮件、审批、聊天合块，避免碎片插入深度块。

### Step 4: 对齐任务与项目

块标题写清可完成的结果（来自 `task-management` 的下一步）；跨里程碑的阶段目标仍由 `project-planning` 定义，日历只落本周可执行块。

### Step 5: 日终对照

未完成块：是拆分问题还是优先级问题？回写任务系统，必要时改明日块。

## Examples

### 半日时间块

```text
09:30–11:30 深度：完成迁移脚本审查
11:30–11:45 缓冲
11:45–12:15 通信批处理
14:00–15:00 会议
15:15–16:15 深度：文档 how-to
```

### 主题块标签

```text
Deep / Admin / Meeting / Break
规则：Deep 不安排可中断会议；Admin 可合并碎片
```

## Validation

- [ ] 固定事件已先放入日历
- [ ] 至少有一个受保护的深度块对应 Top 任务
- [ ] 深度块时长合理（约 60–120 分钟）且有缓冲
- [ ] 浅工作已批处理，而非打散插入
- [ ] 块标题是可完成结果，不是含糊「忙工作」

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 日历塞满零散待办 | 先 `task-management` 澄清，再只上块 Top 项 |
| 无缓冲导致块连锁延误 | 块间 10–15 分钟；会议前后留空隙 |
| 深度块可被任意会议覆盖 | 标「不可移动」并主动拒绝对冲 |
| 块目标过大无法在时长内完成 | 拆成下一步；过大交付回 `project-planning` |
| 忽略笔记/决策沉淀 | 会后要点进 `note-taking`，行动进任务库 |

## References

- 相关技能：`productivity/task-management`、`productivity/note-taking`、`business/project-planning`
