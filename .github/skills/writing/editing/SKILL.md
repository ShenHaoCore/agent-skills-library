---
name: editing
description: >
  Guides editing and proofreading: clarity, concision, consistency, and tone while
  preserving the author's meaning and facts.
  USE FOR: polishing emails, reports, and doc paragraphs; unifying terminology and
  tone; cutting fluff; fixing ambiguity and awkward sentences without inventing new
  persuasive angles or doc architecture from scratch.
  关键词：编辑、润色、校对、editing、改写、proofreading、简洁、语气一致。
  DO NOT USE FOR: inventing marketing angles from scratch (use copywriting);
  structuring new technical docs from zero (use technical-writing).
license: MIT
---

# Editing

在不扭曲原意的前提下提升清晰度、简洁性与一致性。

需要从零搭技术文档结构时用 `writing/technical-writing`；需要从零写转化向文案时用 `writing/copywriting`。

## When to Use

- 润色邮件、报告、文档段落
- 统一术语与语气
- 删减啰嗦、修复病句与歧义
- 在保留事实与约束下改写表达

## When Not to Use

- 从零设计 README / how-to / API 结构 → `writing/technical-writing`
- 从零发明卖点、落地页转化路径 → `writing/copywriting`
- 用户要求改变核心论点、事实或合规表述
- 纯翻译任务且无「润色原文」需求（除非用户明确要求译后润色）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 原文 | Yes | 待编辑文本 |
| 保留约束 | Recommended | 术语、合规、不可改事实 |
| 目标读者与语气 | Recommended | 正式 / 口语等 |
| 篇幅目标 | Optional | 如「压缩到一半」「保持长度」 |

## Workflow

### Step 1: 通读标记

标出歧义、重复、逻辑跳跃、术语不一致。

### Step 2: 按层改写

顺序：结构 → 段落 → 句子 → 用词 → 标点。一句一意；能删则删；适场景被动改主动。

### Step 3: 统一规范

统一专有名词、中英文空格与并列结构；不擅自改事实。

### Step 4: 交付说明

列出「关键改动」与「仍存疑点」；需要结构重建或营销重构时指向对应技能。

## Examples

### 冗长 → 简洁

```text
原文：我们将会在后续的时间里对相关问题进行进一步的优化处理。
改写：我们会继续优化这些问题。
```

### 交付附注格式

```text
关键改动：
- 删重复状语，合并两句为一句
- 「用户们」统一为「用户」

仍存疑：
- 「下周上线」是否已确认？原文未给日期锚点
```

## Validation

- [ ] 原意与不可改事实未被扭曲
- [ ] 歧义、重复、明显病句已处理
- [ ] 术语与语气前后一致
- [ ] 交付含关键改动说明（及存疑点）
- [ ] 未越界去做技术文档架构或营销策略发明

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 改得「更好听」但改了意思 | 对照原句约束；存疑处标出而非臆断 |
| 只改用词不改结构臃肿 | 先并段/拆句，再换词 |
| 术语中英混用无规范 | 建临时对照表并全文统一 |
| 把编辑做成重写产品定位 | 转交 `copywriting` 或请用户确认策略变更 |
| 把编辑做成新文档大纲 | 转交 `technical-writing` |

## References

- 相关技能：`writing/technical-writing`、`writing/copywriting`
