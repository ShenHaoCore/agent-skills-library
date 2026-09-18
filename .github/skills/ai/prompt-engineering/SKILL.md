---
name: prompt-engineering
description: >
  Guides LLM prompt structure: roles, few-shot examples, chain-of-thought,
  output constraints, and common anti-patterns.
  USE FOR: designing or improving system/user prompts; stable JSON/table/template
  output; few-shot style alignment; reducing hallucination and format drift;
  reviewing prompt anti-patterns (vague goals, contradictions, missing rubrics).
  关键词：提示词、prompt、LLM、少样本、few-shot、chain-of-thought、角色设定、
  输出格式、JSON schema、system prompt、幻觉、约束。
  DO NOT USE FOR: training or fine-tuning models; ML infra; general agent /
  skill architecture and routing (use agent-design); human-facing docs
  (use writing/technical-writing).
license: MIT
---

# Prompt Engineering

设计高命中率的提示词：目标清晰、约束明确、示例对齐、输出可解析。

若在设计 Agent / Skill 架构，请改用或联用 `ai/agent-design`；写给人看的文档用 `writing/technical-writing`。

## When to Use

- 编写或优化 system / user prompt
- 需要稳定 JSON / 表格 / 特定模板输出
- 用 few-shot 对齐风格与边界
- 减少幻觉、跑题、格式漂移
- 评审提示词反模式（过长、矛盾、无评价标准）

## When Not to Use

- 训练 / 微调模型、ML 基础设施
- Agent 职责划分、Skill 目录与路由 → `ai/agent-design`
- 面向人类读者的 README / API 文档结构 → `writing/technical-writing`
- 纯润色文法且无提示词目标 → `writing/editing`

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 任务目标与成功标准 | Yes | 「什么样的回答算对」 |
| 目标模型 / 渠道 | Recommended | 上下文长度、是否支持系统角色、工具调用 |
| 失败样例或当前提示 | Optional | 用于迭代约束与 few-shot |
| 期望输出格式 | Recommended | Markdown / JSON schema / 纯代码块等 |

## Workflow

### Step 1: 写清目标与非目标

1. What to do + What not to do；成功标准可检验。
2. 列出禁止事项（编造数据、越权角色、多余前言）。

### Step 2: 结构化提示

推荐块顺序：`Role` → `Task` → `Context` → `Constraints` → `Output format` → `Examples`。

### Step 3: 角色与少样本

1. 角色设定要可操作：「资深 SRE」不如「按严重级别输出排查步骤并给出验证命令」。
2. Few-shot：1–3 个高质量示例，覆盖边界；示例格式必须与最终输出一致。

### Step 4: 推理深度与输出约束

1. 复杂推理可要求分步思考；数学/逻辑可要求检验。
2. 不需要时不要强迫冗长 CoT（浪费 token、泄露推理）。
3. 指定 Markdown 标题、JSON schema、或「只输出代码块」；要求「除 JSON 外无其他文字」时，示例也必须干净。

### Step 5: 迭代与验证

1. 先跑失败样例；把失败变成约束或新示例；保持提示短而硬。
2. 按 Validation 清单检查；需要模式库时读 `references/prompt-patterns.md`。

## Examples

### 基础结构

```text
Role: 你是资深技术编辑，擅长把草稿改成可执行的操作文档。
Task: 重写下方 README 的「安装」一节。
Constraints:
- 保留项目真实命令，不编造 flag
- 中文
- 步骤编号
Output:
- 只输出改写后的 Markdown 小节，不要前言
```

### 少样本对齐分类

```text
将工单标为 billing | technical | other。只输出类别。

示例：
输入：发票金额不对 → billing
输入：登录返回 500 → technical
输入：想夸一下客服 → other

输入：{ticket}
```

### JSON 约束

```text
根据会议记录提取行动项。输出 JSON 数组，schema：
[{"owner":"string","task":"string","due":"YYYY-MM-DD|null"}]
不要输出 Markdown 围栏或其他文字。
```

### 反模式 → 修正

| 反模式 | 修正 |
| --- | --- |
| 「写得好一点」 | 给出读者、长度、语气、必含章节 |
| 又要短又要穷尽一切 | 拆成多步 prompt 或限制 Top N |
| 示例格式与要求不一致 | 示例改成最终格式 |
| 隐藏评价标准 | 明确验收清单 |

## Validation

- [ ] 任务可验收（成功标准明确）
- [ ] 约束无自相矛盾
- [ ] 输出格式唯一且有示例（格式与示例一致）
- [ ] 不要求模型使用未提供的私有数据
- [ ] 失败样例已转化为规则或 few-shot
- [ ] 未强迫不必要的冗长 CoT

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 目标模糊、无验收标准 | 写可检验的成功条件与非目标 |
| 约束互相打架 | 拆 prompt 或排优先级（长度 vs 穷尽） |
| few-shot 与最终格式不一致 | 示例改成最终输出形态 |
| 提示过长淹没任务 | 删冗余背景；硬约束前置 |
| 要求模型编造未知私有信息 | 只基于给定上下文；缺数据则标明 |

## References

- 模式与反模式扩写：`references/prompt-patterns.md`
- 相关技能：`ai/agent-design`、`writing/technical-writing`
