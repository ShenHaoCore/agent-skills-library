---
name: project-planning
description: >
  Guides project planning: milestones, work breakdown, sequencing, buffers, and
  risk tracking.
  USE FOR: project timelines and roadmap slices; WBS and dependencies; critical
  path; sprint/phase plans; risk buffers and fallback scope cuts.
  关键词：项目规划、里程碑、排期、roadmap、WBS、关键路径、sprint planning、
  风险缓冲、完成定义、DoD。
  DO NOT USE FOR: personal GTD habits (use task-management); product problem
  discovery or MVP “do we build it” (use product-thinking); pure calendar
  time-blocking without delivery milestones (use time-blocking).
license: MIT
---

# Project Planning

把目标拆成可交付的里程碑与可执行工作包，并显式管理风险与缓冲。

产品「做不做」、MVP 范围先看 `business/product-thinking`；个人待办习惯看 `productivity/task-management`。

## When to Use

- 制定项目里程碑与路线图切片
- WBS 拆解、依赖与关键路径
- Sprint / 阶段计划与风险缓冲
- 设定检查点与范围降级方案

## When Not to Use

- 产品问题发现、价值取舍、MVP 是否立项 → `business/product-thinking`
- 个人 Inbox / GTD / 日常待办习惯 → `productivity/task-management`
- 仅日历时间块、无交付里程碑 → `productivity/time-blocking`
- Agent / Skill 架构设计 → `ai/agent-design`

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标与完成定义（DoD） | Yes | 里程碑验收物是什么 |
| 约束 | Yes | 日期、人力、范围边界 |
| 工作类型概览 | Recommended | 设计 / 开发 / 测试 / 发布等 |
| 已知风险与依赖 | Optional | 外部团队、供应商、合规窗口 |

## Workflow

### Step 1: 完成定义与里程碑

1. 定义 DoD 与每个里程碑的验收物（可演示、可测试）。
2. 里程碑按价值切片，而非仅按技术层切。

### Step 2: WBS 拆解

1. 拆到可估的工作包（人天级）。
2. 每项有明确负责人假设与「完成长什么样」。

### Step 3: 依赖与关键路径

1. 标注依赖；识别关键路径。
2. 能并行的并行；串行瓶颈显式标出。

### Step 4: 缓冲与降级

1. 缓冲加在风险与关键路径上，而非均匀摊薄所有任务。
2. 设定检查点与降级方案（范围裁剪清单）。
3. 日常执行任务可链到 `task-management`。

## Examples

### 里程碑切片

```text
里程碑 M1（2 周）：可内测的账单草稿
- 设计：发票模板 v1
- 开发：PDF 导出
- 测试：3 条主路径
风险：邮件服务延期 → 降级为仅下载 PDF
缓冲：关键路径 +2 天
```

### 简易依赖表

| 工作包 | 依赖 | 估计 | 关键路径？ |
| --- | --- | --- | --- |
| 模板设计 | — | 2d | 是 |
| PDF 导出 | 模板设计 | 3d | 是 |
| 邮件发送 | PDF 导出 | 2d | 否（可降级） |

### 检查点

```text
W1 结束：模板可预览
W2 中：PDF 主路径可导出
W2 末：内测包 + 已知问题列表；邮件未就绪则切降级
```

## Validation

- [ ] 每个里程碑有可验收物与 DoD
- [ ] WBS 到可估工作包；依赖已标
- [ ] 关键路径已识别；并行项已利用
- [ ] 缓冲在风险处，非平均摊派
- [ ] 有检查点与范围降级清单
- [ ] 未把「做不做」混进排期（应先 `product-thinking`）

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 只有日期没有验收物 | 每里程碑写可演示 / 可测产出 |
| 工作包过大无法估计 | 拆到人天级，含完成标准 |
| 缓冲均匀洒在所有任务 | 集中到风险与关键路径 |
| 无降级方案 | 预写范围裁剪清单 |
| 与产品发现混谈 | 范围先 `product-thinking`，再排期 |
| 用个人 todo 代替项目计划 | 里程碑用本技能；日常用 `task-management` |

## References

- 产品范围与 MVP：`business/product-thinking`
- 执行任务习惯：`productivity/task-management`
- 相关：`productivity/time-blocking`
