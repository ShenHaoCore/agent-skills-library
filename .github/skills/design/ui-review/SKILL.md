---
name: ui-review
description: >
  Guides structured UI design reviews: usability, hierarchy, consistency, spacing,
  color, and accessibility walkthroughs with severity-ranked findings.
  USE FOR: UI/UX walkthrough; design QA before ship; spacing/color/consistency checks;
  a11y quick audit (keyboard, contrast, labels, focus); review of mockups or screenshots.
  关键词：UI 评审、设计走查、可用性、UX review、间距、色彩、可访问性、a11y、
  一致性、spacing、对比度、空状态、错误态。
  DO NOT USE FOR: defining tokens/component libraries from scratch (use design-system);
  brand marketing copy; backend API design; implementing new UI features without review intent.
license: MIT
---

# UI Review

对界面做结构化走查：先任务与可用性，再视觉一致性，最后无障碍。输出可执行问题清单，而非主观品味争论。

设计 token / 组件规范问题交叉加载 `design/design-system`。

## When to Use

- 设计稿或实现稿的 UI/UX 评审
- 上线前视觉与可用性 QA
- 检查间距、层级、色彩对比、组件一致性
- 无障碍（键盘、对比度、语义、焦点）快速审计

## When Not to Use

- 从零定义 token / 组件库规范 → `design/design-system`
- 品牌营销文案或落地页文案 → `writing/copywriting`
- 后端 API / 数据模型设计
- 仅实现功能、无评审意图的纯编码任务

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 页面 / 原型 / 截图 | Yes | 可访问的评审对象 |
| 主用户任务 | Recommended | 至少 1 条主路径与成功标准 |
| 设备与主题 | Optional | 桌面 / 移动；亮 / 暗 |
| 设计系统基准 | Optional | 组件、间距尺度（如 4/8pt）；有则对照 |

## Workflow

### Step 1: 对齐目标

1. 主任务是什么？成功状态如何衡量？
2. 明确设备与主题范围。
3. 若有设计系统，记下间距与组件基准。

### Step 2: 走主路径（可用性）

1. 按真实步骤点一遍：入口 → 填写 → 提交 → 反馈。
2. 记录困惑点、死胡同。
3. 检查空状态 / 错误态 / 加载态 / 权限不足态是否齐全。

### Step 3: 信息层级与一致性

1. 一眼能否看到主 CTA？标题层级是否混乱？
2. 次要操作是否抢主按钮视觉权重？
3. 同义组件是否同款；文案语气、图标、圆角、描边是否统一？

### Step 4: 间距、排版与色彩

1. 间距是否跟网格（如 4/8）？区块节奏是否忽密忽疏？
2. 行长、字号阶梯、对齐是否整齐？
3. 语义色是否滥用；正文/图标对比度是否足够？

### Step 5: 可访问性

1. 键盘可达与焦点可见；图片 alt；表单 label。
2. 触控目标大小（建议 ≥ 44px）。
3. 不只用颜色传达状态。

### Step 6: 输出与分级

按 Critical / Major / Minor 列出问题，附位置与可验证建议。细节清单见 `references/review-checklist.md`。

## Examples

### 走查输出模板

```markdown
# UI Review — Checkout page

## Scope
- Desktop + mobile web
- Primary task: complete payment

## Findings
### Critical
- [ ] 支付按钮在错误态仍可点击且无说明（建议：校验失败时 disable + 行内错误）

### Major
- [ ] 主 CTA 与「返回购物车」视觉权重相近（建议：提高主按钮对比与字重）

### Minor
- [ ] 卡片内边距 12/16 混用（建议：统一 16）

## A11y
- [ ] 焦点环被 `outline: none` 去掉且无替代
- [ ] 价格颜色对比不足（浅灰 on 白）
```

### 快速清单（可复制）

```text
Usability: 主路径 / 空状态 / 错误态 / 加载态 / 权限不足态
Hierarchy: 标题阶梯 / 主 CTA 唯一
Consistency: 组件 / 间距 / 文案
Visual: 对齐 / 对比 / 图标
A11y: 键盘 / 焦点 / label / 对比度 / 触控 ≥ 44px
```

### 间距问题示例描述

> 「筛选区与结果列表间距 8px，但列表与底栏 32px，节奏断裂；建议区块间距统一 24px，组件内 16px。」

## Validation

- [ ] 有明确主任务与设备范围
- [ ] 问题按 Critical / Major / Minor 分级，非同等堆砌
- [ ] 每条建议可验证（位置 + 做法），非「更有质感」类主观词
- [ ] 覆盖空 / 加载 / 错误态（若适用）
- [ ] 至少检查键盘焦点与对比度（若适用）
- [ ] token/组件规范缺口已指向或交叉 `design/design-system`

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 只谈「好不好看」无任务上下文 | 先走主路径，再谈视觉 |
| 一次提出 50 条同等优先级 | 强制 Critical / Major / Minor |
| 忽略错误态与空状态 | Workflow Step 2 强制检查 |
| 建议无法验证 | 写具体度量（间距 px、对比度、禁用规则） |
| 把系统设计当走查 | 规范建设交给 `design-system` |

## References

- 完整检查表：`references/review-checklist.md`
- 相关技能：`design/design-system`
---
