---
name: task-management
description: >
  Guides personal and team task management: capture, clarify, organize, GTD-style
  lists, kanban WIP limits, prioritization, and daily/weekly review.
  USE FOR: unifying scattered todos; GTD or lightweight kanban; Eisenhower / deadline
  conflicts; choosing one trusted task system; daily and weekly review habits.
  关键词：任务管理、GTD、todo、看板、优先级、inbox、每日回顾、Eisenhower、任务收集。
  DO NOT USE FOR: multi-month project roadmaps (use business/project-planning);
  calendar time-block layout details (use time-blocking); note/PKM structure
  (use note-taking); PM certification curricula.
license: MIT
---

# Task Management

建立「收集 → 澄清 → 组织 → 回顾 → 执行」的闭环，让任务可见、可排序、可完成。

细排日程时交叉加载 `productivity/time-blocking`；知识笔记用 `productivity/note-taking`；
项目级里程碑与关键路径用 `business/project-planning`。

## When to Use

- 任务散落在聊天、邮件、大脑，需要统一收集
- 不知道先做哪件：优先级 / Eisenhower / 截止日冲突
- 想落地 GTD 或轻量看板
- 建立每日 / 每周回顾节奏
- 选择待办工具（而非纠结工具本身）

## When Not to Use

- 多月路线图、里程碑、WBS、关键路径 → `business/project-planning`
- 已在日历上画深度工作块 / 主题日 → `productivity/time-blocking`
- 永久笔记、Zettelkasten、PKM 结构 → `productivity/note-taking`
- 用换工具代替回顾与澄清

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 可信任务库 | Yes | 唯一 Inbox/待办系统（应用、看板或 Markdown） |
| 原始事项列表 | Recommended | 聊天摘录、脑中清单、邮件待办等 |
| 日历约束摘要 | Optional | 已知会议/截止日，便于排优先级 |
| 回顾时长承诺 | Optional | 每日 5–10 分钟；每周更长一次 |

## Workflow

### Step 1: 收集（Capture）

所有事项先进入 Inbox，不当场排序。规则：不进系统 = 不可靠。

### Step 2: 澄清（Clarify）

对 Inbox 每项问：是否可行动？下一步物理动作是什么？  
2 分钟内能做完 → 立刻做；否则变成任务 / 项目 / 资料 / 将来也许。

### Step 3: 组织（Organize）

- 列表建议：Inbox、Next、Waiting、Projects、Someday。
- 或看板：`Backlog → Doing（限时在制）→ Done`。
- 进行中任务限制数量（如个人 WIP ≤ 3）。

### Step 4: 优先级

| 象限 | 动作 |
| --- | --- |
| 紧急且重要 | 今日 |
| 重要不紧急 | 日程块（链到 `time-blocking`） |
| 紧急不重要 | 委托或压缩 |
| 都不 | 删除或 Someday |

项目级里程碑与依赖仍归 `business/project-planning`；本技能只落到可执行下一步。

### Step 5: 每日 / 每周回顾

- **每日**：清空 Inbox；核对日历；选出今日最重要 1–3 项；跟进 Waiting。
- **每周**：扫 Projects 是否有下一步；清理 Done；目标是否对齐。模板见 `references/review-templates.md`。

### Step 6: 工具原则

先流程后工具；能同步、能搜索、摩擦低即可。避免多个互不同步的待办库。

## Examples

### 澄清对话（可照做）

```text
原始：把网站搞好
澄清后：
- 项目：官网改版
- 下一步：列出首页信息架构草案（30 分钟）
- 等待：设计稿（等 @Alice）
```

### 轻量看板列定义

| 列 | 规则 |
| :--- | :--- |
| Inbox | 未澄清 |
| Ready | 已有明确下一步 |
| Doing | WIP ≤ 3 |
| Waiting | 有明确等待对象与跟进日 |
| Done | 本周保留，周末归档 |

### 每日回顾清单

```text
- [ ] Inbox 清零
- [ ] 日历冲突已处理
- [ ] 写下今日 Top 1–3
- [ ] Waiting 是否需要催办
- [ ] 将超过 2 天的 Doing 拆分或降级
```

### Eisenhower 速判

|  | 紧急 | 不紧急 |
| :--- | :--- | :--- |
| **重要** | 立即做 | 安排时间块 |
| **不重要** | 委托/缩短 | 删除 |

## Validation

- [ ] 仅一个可信 Inbox / 任务库在用
- [ ] Inbox 项已澄清为下一步动作、项目、资料或 Someday
- [ ] Doing / 今日焦点有 WIP 上限（如 ≤ 3）
- [ ] Waiting 有对象与跟进日
- [ ] 已约定每日或每周回顾（可引用模板）

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 只有 deadline 没有下一步 | 写成可执行物理动作 |
| Doing 列堆满 | 强制 WIP；拆分或降级 |
| 用日历当待办垃圾桶 | 任务进任务库；日历留给时间块（`time-blocking`） |
| 工具换来换去不回顾 | 固定系统 + 套用回顾模板 |
| 把项目排期当成个人 GTD | 里程碑交给 `project-planning`，这里只要下一步 |

## References

- 回顾模板：`references/review-templates.md`
- 相关技能：`productivity/time-blocking`、`productivity/note-taking`、`business/project-planning`
