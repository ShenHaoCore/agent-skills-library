---
name: technical-writing
description: >
  Guides technical documentation: audience-focused structure, how-to/tutorial/reference,
  code sample standards, API docs, and README authoring.
  USE FOR: writing or rewriting README, how-to guides, tutorials, concept explanations,
  API references; structuring docs by reader task; normalizing command/code blocks.
  关键词：技术写作、文档、documentation、API 文档、README、面向读者、how-to、操作指南、
  tutorial、reference。
  DO NOT USE FOR: marketing or landing-page copy (use copywriting); grammar-only polish
  without technical structure (use editing); LLM prompt engineering (use ai/prompt-engineering).
license: MIT
---

# Technical Writing

写出读者能快速完成任务的技术文档：结构清晰、步骤可执行、示例可复制。

需要润色措辞时交叉加载 `writing/editing`；需要说服性产品文案时用 `writing/copywriting`；
LLM 提示词技巧用 `ai/prompt-engineering`。

## When to Use

- 撰写或重写 README、贡献指南、架构说明
- 编写 how-to、教程、概念解释、API / 配置参考
- 规范文档中的代码示例与命令块
- 按读者角色（初学者 / 贡献者 / 运维）调整信息架构

## When Not to Use

- 落地页、slogan、CTA、营销卖点 → `writing/copywriting`
- 仅润色已有段落（不改文档类型与结构）→ `writing/editing`
- 设计 Agent / LLM 提示词 → `ai/prompt-engineering`
- 无真实产品信息、无法给出可运行命令时的空壳「文档模板填空」表演

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 读者与成功标准 | Yes | 谁读完后能完成什么任务 |
| 文档类型意图 | Recommended | README / How-to / Tutorial / Reference / Explanation |
| 真实命令或代码 | Recommended | 可运行路径；避免伪命令 |
| 产品/模块名与版本约束 | Optional | 有则写入前置条件，避免「目前」「最近」无锚点 |

## Workflow

### Step 1: 先定读者与任务

1. 一句话：谁在什么情境下要完成什么。
2. 删掉与该任务无关的背景。

### Step 2: 选对文档类型

| 类型 | 用途 |
| --- | --- |
| **README** | 是什么、为何用、最快上手、链到深文档 |
| **How-to** | 面向目标的步骤清单 |
| **Tutorial** | 带学习目标的引导路径 |
| **Reference** | 完整、可扫描的 API / 配置说明 |
| **Explanation** | 设计原因与权衡（少步骤） |

细节见 `references/doc-types.md`。

### Step 3: 套结构模板

标题 → 一段话目的 → 前置条件 → 步骤 → 验证 → 故障排除 → 下一步。

### Step 4: 步骤可执行

- 一步一个动作；命令用带语言标签的代码块；标明工作目录与预期输出。

### Step 5: 代码与 API 规范

- 示例可复制、可运行（或明确标为示意）；正确路径放正文，常见错误放「故障排除」。
- 隐藏密钥；占位符用 `<token>` 等形式。
- API 每个端点：方法、路径、认证、参数、示例请求/响应、错误码；字段表含名称、类型、必填、说明。

### Step 6: 降噪与验证

- 避免空洞形容词；用具体名称与数字；版本或日期锚定变更。
- 按 Validation 清单自检后交付。

## Examples

### How-to 骨架

```markdown
# 如何发布新版本

将通过 tag 触发 CI 发布 npm 包。

## 前置条件
- 维护者权限
- 本地 `main` 已同步

## 步骤
1. 更新 CHANGELOG
2. bump 版本并打 tag
3. 推送 tag 并确认 Actions 通过

## 验证
- npm 页面出现新版本号
```

### README 最短可用结构

```markdown
# project-name

一句话说明。

## 快速开始
## 安装
## 用法
## 配置
## 贡献
## License
```

### API 条目示例

```markdown
### `POST /v1/sessions`

创建会话。需要 `Authorization: Bearer <token>`。

| 字段 | 类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| `name` | string | 是 | 显示名称 |
| `ttlSec` | number | 否 | 默认 3600 |
```

### 命令示例格式

```bash
cd apps/api
dotnet test --filter FullyQualifiedName~UserService
```

## Validation

- [ ] 读者与任务在文首清晰
- [ ] 文档类型与读者目标匹配
- [ ] 前置条件完整
- [ ] 步骤可照做；有验证方式
- [ ] 代码/命令可复制且安全（无密钥）
- [ ] 链接到相关深文档，而非重复粘贴长文

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 写成产品宣传而非任务文档 | 改用 how-to/reference；营销交给 `copywriting` |
| 步骤不可执行（缺目录/预期输出） | 补工作目录、命令、成功判据 |
| 伪代码冒充可运行示例 | 标示意，或给出真实最小可运行片段 |
| 泄露密钥 / 真实 token | 占位符；审查命令与响应样例 |
| 「目前」「最近」无版本锚点 | 写版本号或日期 |

## References

- 文档类型与语气：`references/doc-types.md`
- 相关技能：`writing/editing`、`writing/copywriting`、`ai/prompt-engineering`
