---
name: product-thinking
description: >
  Guides product thinking: problem framing, user value, MVP scope, and
  prioritization.
  USE FOR: clarifying product direction; writing problem statements and user
  stories; MVP tradeoffs; value proposition and prioritization decisions.
  关键词：产品思维、MVP、需求、user story、产品决策、价值主张、优先级排序、
  问题陈述、验收标准。
  DO NOT USE FOR: detailed engineering timelines or WBS (use project-planning);
  personal GTD / todo boards (use task-management); UI visual QA (use ui-review);
  marketing landing copy only (use copywriting).
license: MIT
---

# Product Thinking

从用户问题出发定义价值、范围与优先级，避免「功能清单驱动」。

交付排期与里程碑见 `business/project-planning`。

## When to Use

- 梳理新产品 / 新功能要不要做
- 写问题陈述、用户故事、验收标准
- 压缩 MVP 范围、做取舍
- 用价值 × 置信度 / 成本做优先级，并写出「不做」清单

## When Not to Use

- 已定范围后的里程碑、WBS、关键路径与缓冲 → `business/project-planning`
- 个人待办 / 看板习惯 → `productivity/task-management`
- 界面视觉与可用性走查 → `design/ui-review`
- 仅写落地页营销文案 → `writing/copywriting`

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标用户假设 | Recommended | 可粗；谁、场景、痛点 |
| 约束 | Recommended | 时间、合规、技术边界 |
| 现状替代方案 | Optional | 用户今天怎么凑合 |
| 候选功能 / 需求列表 | Optional | 待压缩与排序的清单 |

## Workflow

### Step 1: 问题陈述

写清：谁、场景、痛苦、现状替代方案。避免一上来列功能。

### Step 2: 成功指标

定义可测量的行为或业务指标（读完/用完能验证什么）。

### Step 3: 假设与最小实验

1. 列出方案假设；标出最危险假设。
2. 用最小实验验证，再扩大范围。

### Step 4: MVP 与优先级

1. MVP：只保留验证核心价值的路径；其余进 Later。
2. 优先级：价值 × 置信度 / 成本；明确不做清单。
3. 范围稳定后，排期交给 `project-planning`。

## Examples

### MVP 切片

```text
问题：兼职设计者开票流程超过 30 分钟且易错
MVP：模板 + 一键 PDF + 邮件发送
非目标：多币种税务引擎（Later）
验收：新用户 10 分钟内发出第一张发票
```

### 用户故事骨架

```text
作为 <角色>，我想 <能力>，以便 <价值>。
验收：
- [ ] <可观察行为 1>
- [ ] <可观察行为 2>
```

### 优先级速判

| 项 | 价值 | 置信度 | 成本 | 结论 |
| --- | --- | --- | --- | --- |
| 模板开票 | 高 | 高 | 低 | MVP |
| 多币种税务 | 中 | 低 | 高 | Later |

## Validation

- [ ] 有问题陈述（谁 / 场景 / 痛点 / 替代方案）
- [ ] 成功指标可测量
- [ ] MVP 路径能验证核心价值；有明确非目标
- [ ] 优先级有依据；「不做」清单已写出
- [ ] 未用功能清单替代用户问题

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 功能清单驱动 | 先写问题与价值，再列能力 |
| MVP 塞满 Later 项 | 只留验证核心假设的路径 |
| 无验收标准 | 写成可观察行为或业务数字 |
| 与排期混谈 | 范围用本技能；日程用 `project-planning` |
| 忽略替代方案 | 写清用户今天怎么解决，证明改进空间 |

## References

- 排期与里程碑：`business/project-planning`
- 相关：`writing/copywriting`、`productivity/task-management`
