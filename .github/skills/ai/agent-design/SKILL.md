---
name: agent-design
description: >
  Guides AI agent architecture: tool use, skill boundaries, routing, memory,
  and evaluation loops.
  USE FOR: designing agents or skill libraries; multi-step tool-calling workflows;
  skill description and trigger strategy; multi-agent handoff; defining success
  criteria and regression evals.
  关键词：Agent 设计、工具调用、skill 设计、multi-agent、路由、记忆、评测、
  技能边界、触发策略。
  DO NOT USE FOR: single-prompt wording tweaks only (use prompt-engineering);
  generic product strategy without agent context (use product-thinking);
  personal task habits (use task-management).
license: MIT
---

# Agent Design

设计可扩展、可路由、可评测的 Agent 与 Skill 体系。

提示词措辞细节见 `ai/prompt-engineering`。本仓库导航见根目录 `AGENTS.md`；关键词路由见 `docs/skill-triggers.md`。

## When to Use

- 规划 Agent 职责与工具边界
- 设计 Skill 目录、description 与触发策略
- 多 Agent 分工与交接
- 定义成功标准与回归用例
- 为本仓库或同类技能库增删技能、写路由规则

## When Not to Use

- 仅优化单条 prompt 措辞 / few-shot / JSON 约束 → `ai/prompt-engineering`
- 产品「做不做」、MVP 价值取舍（无 Agent 上下文）→ `business/product-thinking`
- 个人待办 / GTD 习惯 → `productivity/task-management`
- 纯工程排期与里程碑 → `business/project-planning`

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 用户任务族 | Yes | Agent 要覆盖的场景范围（不是「什么都会」） |
| 运行时能力 | Recommended | 可用工具、skills、上下文长度、是否多 Agent |
| 现有技能 / 目录 | Optional | 如 `.github/skills/`、description 样例 |
| 成功标准 / 评测集 | Recommended | 固定任务与可观察输出 |

## Workflow

### Step 1: 划定 Agent 边界

1. 写清输入 / 输出与禁止事项。
2. 列出不做的任务族，避免万能 Agent。

### Step 2: 拆成技能（单一职责）

1. 每个 skill 只覆盖一类明确任务；目录名小写 + 连字符。
2. `description` 含 What / When（USE FOR）/ Keywords（中英）/ NOT for（DO NOT USE）。
3. `name` 与技能目录名一致；细节放 `references/`，正文保持可扫描。

### Step 3: 路由策略

1. 先定领域，再定技能；优先精确匹配用户明确技术/场景。
2. 关键词歧义时查触发表（本仓库：`docs/skill-triggers.md`）。
3. 默认单技能加载；跨领域时再交叉引用，勿一次灌入整包。
4. 仓库级导航：根目录 `AGENTS.md`。

### Step 4: 工具与记忆

1. 工具用最小集合；失败可重试且有用户可见错误。
2. 记忆区分会话短期 vs 持久偏好；敏感数据不入库。

### Step 5: 评测与回归

1. 固定任务集；改技能或路由后跑回归。
2. 记录失败样例，反馈到 description / 触发词 / 工作流。

## Examples

### 技能粒度

```text
坏：一个 skill 叫 "coding" 包揽所有语言
好：programming/csharp/async-patterns 只解决 C# 异步
```

### 路由

```text
用户提「EF 迁移」→ 只加载 ef-core-migrations
用户提「提示词 JSON 约束」→ prompt-engineering（非本技能）
用户提「Skill 目录与触发」→ agent-design
```

### description 骨架

```text
Guides <what>.
USE FOR: <concrete triggers>.
关键词：<中英关键词>.
DO NOT USE FOR: <相邻技能与排除项>.
```

## Validation

- [ ] Agent 有明确输入/输出与禁止事项
- [ ] 每个 skill 单一职责；description 含 USE FOR / 关键词 / DO NOT USE
- [ ] 路由默认单技能；歧义有触发表或回退策略
- [ ] 工具集最小；失败路径对用户可见
- [ ] 记忆分层且无敏感数据滥存
- [ ] 有固定评测集；改动后可回归

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 万能 skill / 巨型 Agent | 按任务族拆分；写清 NOT for |
| description 无关键词或排除项 | 补齐 USE FOR / 关键词 / DO NOT USE |
| 一次加载整领域 | 精确匹配；默认单技能 |
| 工具过多且无失败处理 | 最小集 + 重试 + 用户可见错误 |
| 改技能不跑回归 | 固定 5–10 条任务集，改一处测一处 |
| 把提示词细节塞进本技能 | 联用或改用 `prompt-engineering` |

## References

- 提示词：`ai/prompt-engineering`
- 仓库导航：根目录 `AGENTS.md`
- 关键词映射：`docs/skill-triggers.md`
