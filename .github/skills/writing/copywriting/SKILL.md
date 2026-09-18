---
name: copywriting
description: >
  Guides product and marketing copy: value propositions, landing page copy, CTAs,
  and concise slogans under real capability limits.
  USE FOR: landing/product page copy; email subject/body; slogans; feature bullets;
  CTA button labels; short brand-voice variants for persuasive goals.
  关键词：文案、copywriting、营销文案、落地页、slogan、CTA、价值主张、卖点。
  DO NOT USE FOR: API/README technical docs (use technical-writing); grammar-only
  editing without persuasive goals (use editing); inventing false product claims.
license: MIT
---

# Copywriting

写清楚「给谁、解决什么、为何现在行动」的营销与产品文案。

技术文档用 `writing/technical-writing`；仅润色已有文案措辞用 `writing/editing`。

## When to Use

- 落地页、产品页、邮件标题与正文
- Slogan、功能卖点、CTA 按钮文案
- 统一品牌语气下的短文案变体（稳重 / 直接 / 友好等）

## When Not to Use

- README、how-to、API 参考 → `writing/technical-writing`
- 不改说服目标、只改病句与啰嗦 → `writing/editing`
- 无法核实的能力夸大、虚假承诺或违规宣称
- 长篇技术架构说明（非转化向）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标受众与痛点 | Yes | 哪怕一句话：给谁、什么痛 |
| 产品真实能力边界 | Yes | 禁止超出边界的承诺 |
| 渠道与限制 | Recommended | 字数、版位、合规要求 |
| 品牌语气偏好 | Optional | 正式 / 直接 / 友好等 |

## Workflow

### Step 1: 写价值主张

人群 + 问题 + 结果 + 差异（一句可扫描）。

### Step 2: 搭转化结构

注意 → 问题 → 证据/利益 → CTA；删形容词堆砌。每个区块一个信息点；短句与小标题便于扫描。

### Step 3: 打磨 CTA

用具体动词（如「免费创建第一张发票」优于空泛的「提交」，视场景而定）。主 CTA 唯一，次 CTA 降权。

### Step 4: 给出语气变体

交付 2–3 个语气变体供选择；标出不可改的事实约束。

### Step 5: 自检

按 Validation 核对真实性、扫描性与 CTA 清晰度。

## Examples

### 价值主张与 CTA

```text
价值主张草稿：
面向兼职创作者的发票工具——5 分钟出账，少追款，多做创作。

CTA：
- 主：免费创建第一张发票
- 次：查看 2 分钟演示
```

### 区块信息点（落地页）

```text
Hero：人群 + 结果一句话 + 主 CTA
痛点：2–3 条具体场景（非空形容词）
证据：数字、流程、对比（须真实）
CTA 带：重复主行动，降低次要噪音
```

## Validation

- [ ] 价值主张含人群、问题、结果（及可选差异）
- [ ] 承诺未超出产品真实能力
- [ ] 每区块一个信息点；可扫描
- [ ] 主 CTA 具体、唯一
- [ ] 提供至少 2 个语气变体（若用户需要选择）

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 形容词堆砌无证据 | 换成具体结果、数字或可验证对比 |
| 写成技术说明书 | 转到 `technical-writing`，或只保留转化所需事实 |
| CTA 含糊（「了解更多」「提交」） | 写用户得到的下一步动作 |
| 虚假或越界承诺 | 回退到已确认能力边界 |
| 主次 CTA 视觉/文案权重相同 | 明确一个主行动 |

## References

- 相关技能：`writing/technical-writing`、`writing/editing`
