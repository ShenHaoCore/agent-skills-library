---
name: design-system
description: >
  Guides design system fundamentals: tokens, component APIs, spacing scales,
  theming, and documentation for reusable UI consistency.
  USE FOR: defining color/type/space/radius/shadow tokens; component library rules
  (variants, sizes, states); design system docs and contribution/versioning;
  reviewing token naming and reuse boundaries.
  关键词：设计系统、design system、token、组件库规范、间距尺度、主题、变体、
  设计 token、spacing scale。
  DO NOT USE FOR: one-off UI visual QA / walkthrough (use ui-review);
  brand marketing copy; implementing backend APIs; ad-hoc page polish without system intent.
license: MIT
---

# Design System

用 token 与组件约定保证产品界面可扩展、一致性可执行。

单页走查 / 可用性 QA 交叉加载 `design/ui-review`。

## When to Use

- 定义色板、字体、间距、圆角、阴影等 token
- 规范按钮 / 表单 / 对话框等组件 API（变体、尺寸、状态）
- 写设计系统文档与贡献 / 版本规则
- 审查现有系统是否可复用、是否雪花样式泛滥

## When Not to Use

- 单页 UI 走查、上线前视觉 QA → `design/ui-review`
- 品牌营销文案 → `writing/copywriting`
- 后端 API / 服务设计
- 只改一处样式、无意沉淀为系统约定

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 平台 | Recommended | Web / iOS / Android（或跨端约束） |
| 品牌色或参考产品 | Recommended | 起步色板与语气 |
| 现有组件清单 | Optional | 按钮、表单、对话框等已有实现 |
| 主题需求 | Optional | 亮 / 暗 / 多品牌 |

## Workflow

### Step 1: 先 token 后组件

1. 建立核心 token：`color` / `space` / `type` / `radius` / `shadow`。
2. 间距采用有限尺度（如 4 的倍数）；避免任意像素。
3. 语义色与品牌色分层（`color.text` vs `color.brand`）。

### Step 2: 组件 API

1. 分清变体与状态（hover / disabled / loading / focus）。
2. 禁止一次性雪花样式；新样式先问「能否用现有变体」。
3. 尺寸阶梯有限（如 sm / md / lg）。

### Step 3: 文档与版本

1. 每个组件文档：何时用、何时不用、无障碍注意、代码示例。
2. 变更走版本；破坏性变更附迁移说明。
3. 贡献规则：谁可加 token、如何命名、评审门槛。

### Step 4: 与走查衔接

实现落地后用 `design/ui-review` 验证一致性与 a11y；系统缺口回到本技能补 token/API。

## Examples

### 间距与按钮 token

```text
space.xs = 4
space.sm = 8
space.md = 16
space.lg = 24

Button variants: primary | secondary | ghost
Button sizes: sm | md | lg
Button states: default | hover | focus | disabled | loading
```

### 文档片段模板

```markdown
## Button / primary
- Use for: 页面唯一主操作
- Do not use for: 次要导航、危险操作（用 danger）
- A11y: 禁用时保留焦点可达说明；对比度 ≥ AA
```

## Validation

- [ ] token 覆盖 color / space / type（至少），间距为有限尺度
- [ ] 组件有变体、尺寸、状态，无未文档化的一次性样式鼓励
- [ ] 文档含何时用 / 何时不用 / a11y
- [ ] 破坏性变更有版本与迁移说明（若已发布）
- [ ] 单页问题已分流到 `design/ui-review`，未混在系统设计里

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 先画页面再补 token | Workflow：token → 组件 → 页面 |
| 无限间距值 | 固定 scale（4/8 倍数） |
| 变体爆炸 | 合并语义相近变体；新变体要文档理由 |
| 无版本纪律 | 破坏性变更必须迁移说明 |
| 用系统设计替代走查 | 落地一致性交给 `ui-review` |

## References

- 相关技能：`design/ui-review`
---
