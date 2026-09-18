---
name: note-taking
description: >
  Guides effective note-taking and lightweight PKM: capture layers, atomic notes,
  linking, progressive summarization, and Zettelkasten-style workflows.
  USE FOR: meeting/reading/study notes; migrating messy notes to a linkable vault;
  designing tags, links, and summary layers; keeping notes retrievable for future self.
  关键词：笔记、note-taking、Zettelkasten、知识管理、PKM、渐进摘要、双链。
  DO NOT USE FOR: task/GTD execution (use task-management); calendar time-blocking
  (use time-blocking); multi-month delivery roadmaps (use business/project-planning).
license: MIT
---

# Note Taking

建立可检索、可连结的笔记系统，让笔记服务未来的自己。

从笔记长出的行动项立刻转入 `productivity/task-management`；要占用日历的专注时段用
`productivity/time-blocking`；多里程碑交付计划用 `business/project-planning`。

## When to Use

- 会议 / 读书 / 学习笔记方法
- 从散乱笔记迁到可链接的知识库
- 设计标签、链接与摘要层次
- 区分闪念、文献笔记与永久笔记

## When Not to Use

- 待办澄清、看板、GTD 回顾 → `productivity/task-management`
- 在日历上安排深度工作块 → `productivity/time-blocking`
- 项目里程碑与排期 → `business/project-planning`
- 把笔记库当成第二个待办系统（行动项应外移）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 笔记工具 | Yes | Markdown 仓库、Notion、Obsidian 等任一 |
| 笔记来源或原文 | Recommended | 会议记录、读书摘录、闪念 Inbox |
| 组织偏好 | Optional | 偏链接 / 偏标签 / 少量文件夹 |

## Workflow

### Step 1: 分离图层

Inbox 闪念 / 文献笔记 / 永久笔记分开；先捕获后整理。

### Step 2: 写原子永久笔记

一条永久笔记只表达一个观点，用自己的话写；注明来源与日期。

### Step 3: 用链接连接

用双链或显式链接连接相关笔记；少依赖深层文件夹作为唯一导航。

### Step 4: 渐进摘要

加粗 → 高亮 → 摘要区；避免反复重写全文。

### Step 5: 行动外移

笔记中的任务立刻丢进任务系统（`task-management`）；需要整块时间执行时再链到 `time-blocking`。

## Examples

### 永久笔记卡片

```markdown
# 永久笔记：技能 description 决定发现率
- 观点：Agent 靠 description 路由，而非文件夹名
- 链接：[[agent-design]] [[prompt-engineering]]
- 来源：团队实践 2026-03
```

### 会议笔记分流

```text
会议原始记录 → 文献/会议笔记（保留决策与上下文）
可执行项 → 任务系统（下一步 + 负责人）
可复用观点 → 一条条永久笔记并回链会议笔记
```

## Validation

- [ ] 闪念 / 文献 / 永久图层可区分
- [ ] 永久笔记观点单一、用自己的话
- [ ] 相关笔记有链接（不只靠深文件夹）
- [ ] 行动项已进入任务系统，未滞留在笔记里当「假待办」
- [ ] 摘要层次存在（加粗/高亮/摘要），而非无限重写

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 只收藏不消化 | 用自己的话写永久笔记 |
| 深层文件夹迷宫 | 优先链接与少量入口页 |
| 笔记里堆待办 | 外移到 `task-management` |
| 反复全文重写 | 用渐进摘要图层 |
| 与项目排期混在一处 | 交付计划交给 `project-planning` |

## References

- 相关技能：`productivity/task-management`、`productivity/time-blocking`、`business/project-planning`
