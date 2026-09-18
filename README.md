# agent-skills-library

**智能体技能库** — 面向任何人的通用型 AI Agent Skills 集合。

适配 [GitHub Copilot](https://docs.github.com/en/copilot)（VS Code）、[Claude Code](https://docs.anthropic.com/en/docs/claude-code) 以及其他支持 Agent Skills 规范的 AI 编程工具。

## 项目简介

本仓库存放可复用的 Agent Skills：每个技能是一个独立文件夹，用 `SKILL.md` 告诉 AI「做什么、何时用、怎么做」。

核心目标：

1. **通用**：不绑定单一语言、技术栈或行业
2. **多技能**：覆盖编程、写作、效率、设计、运维、数据、AI、商业等领域
3. **可扩展**：按「领域 /（子领域）/ 技能名」两层或三层组织，新增无需改整体结构
4. **可发现**：通过 `description`、`AGENTS.md`、`docs/skill-triggers.md` 让 AI 快速路由

## 目录结构

```text
agent-skills-library/
├── README.md                 # 给人看的说明
├── AGENTS.md                 # 给 AI 看的顶层导航
├── LICENSE                   # MIT
├── docs/
│   └── skill-triggers.md     # 关键词 → 技能映射
├── .github/
│   └── skills/               # Copilot 默认识别的技能根目录
│       ├── programming/
│       ├── writing/
│       ├── productivity/
│       ├── design/
│       ├── devops/
│       ├── data/
│       ├── ai/
│       └── business/
└── .vscode/
    └── settings.json         # 可选：额外技能目录配置
```

约定：

| 层级 | 含义 | 示例 |
| :--- | :--- | :--- |
| 第一层 | 领域包 | `programming/`、`writing/` |
| 第二层 | 子领域或技能组（可选） | `csharp/`、`python/` |
| 第三层 | 具体技能（小写 + 连字符） | `async-patterns/` |
| 必需文件 | 技能入口 | `SKILL.md` |
| 可选 | 按需加载 | `references/`、`scripts/`、`assets/` |

若某领域无需二级分组，也可使用 `领域/技能名/`。

## 如何安装

### GitHub Copilot（VS Code）

1. 克隆本仓库到本地，或将其作为子模块 / 工作区文件夹加入
2. 确保技能位于 `.github/skills/`（本仓库默认路径）
3. 打开 VS Code，启用 GitHub Copilot Chat
4. 在 Chat 中通过 `@` 或技能选择器选用技能（具体 UI 以当前 Copilot 版本为准）

可选：本仓库 `.vscode/settings.json` 配置了：

```json
{
  "chat.skillsLocations": [
    ".github/skills"
  ]
}
```

> **说明**：`chat.skillsLocations` 是否可用取决于 VS Code / Copilot 版本，**以实际版本为准**。若设置无效，保持技能在 `.github/skills/` 即可。

### Claude Code

1. 克隆本仓库
2. 按 Claude Code 文档将技能目录加入其 skills 搜索路径，或把需要的技能复制/软链到 Claude Code 识别的 skills 目录
3. 在会话中通过技能名或描述触发

不同工具的安装路径可能不同，原则是：**让运行时能发现含 `SKILL.md` 的技能文件夹**。

## 如何使用技能

1. **人类**：用自然语言描述任务；可先查 [技能清单](#技能清单) 或 `docs/skill-triggers.md`
2. **AI**：读取技能的 `description` 判断是否适用 → 加载 `SKILL.md` → 按需读取 `references/`
3. **协作提示**：复杂仓库可先让 AI 读根目录 `AGENTS.md` 建立领域地图

示例：

- 「帮我写一个可取消的 C# 异步下载」→ `programming/csharp/async-patterns`
- 「生成生产环境可用的 EF Core 迁移 SQL」→ `programming/csharp/ef-core-migrations`
- 「按技术写作规范改这份 README」→ `writing/technical-writing`
- 「做一轮 UI 走查」→ `design/ui-review`

## 如何新增技能

1. 选定领域（必要时新建领域目录）
2. 创建目录：`.github/skills/<domain>/[<subdomain>/]<skill-name>/`
3. 编写 `SKILL.md`（YAML Frontmatter + 正文）
4. 细节放入 `references/`，保持 `SKILL.md` 简洁（建议 < 500 行）
5. 更新本 README 技能清单与 `docs/skill-triggers.md`
6. （可选）在 `AGENTS.md` 路由表示例中补充一行

`SKILL.md` Frontmatter 最低要求：

```yaml
---
name: skill-name          # 必须与目录名完全一致
description: >-
  What... Use when... 关键词：... NOT for...
license: MIT
---
```

`description` 必须包含：**What**、**When**、**Keywords（中英）**、**NOT for**。

## 如何发布到 GitHub

```bash
git init
git add .
git commit -m "chore: initial agent skills library"
git branch -M main
git remote add origin https://github.com/<your-username>/agent-skills-library.git
git push -u origin main
```

建议在仓库 About 中填写简介，并在 Topics 加上 `agent-skills`、`github-copilot`、`claude-code` 等标签，便于发现。

## 技能清单

### programming

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| async-patterns | `programming/csharp/async-patterns` | C# async/await 最佳实践 |
| modern-csharp | `programming/csharp/modern-csharp` | 现代 C# 语言特性（record / NRT / 模式匹配等） |
| naming-conventions | `programming/csharp/naming-conventions` | 类型/成员/异步/接口/字段等命名规范 |
| code-style | `programming/csharp/code-style` | EditorConfig、dotnet format、分析器与构建强制 |
| ef-core-migrations | `programming/csharp/ef-core-migrations` | EF Core 数据库迁移 |
| pythonic-code | `programming/python/pythonic-code` | Pythonic 风格与反模式 |
| pandas-data | `programming/python/pandas-data` | Pandas 数据分析 |
| modern-js | `programming/javascript/modern-js` | 现代 JavaScript 写法 |

### writing

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| technical-writing | `writing/technical-writing` | 技术文档与 README/API 文档 |
| copywriting | `writing/copywriting` | 营销与产品文案 |
| editing | `writing/editing` | 润色、校对与改写 |

### productivity

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| task-management | `productivity/task-management` | 任务收集、优先级与 GTD |
| note-taking | `productivity/note-taking` | 笔记与知识整理 |
| time-blocking | `productivity/time-blocking` | 时间块与日程规划 |

### design

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| ui-review | `design/ui-review` | UI 走查与可用性清单 |
| design-system | `design/design-system` | 设计系统与组件规范 |

### devops

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| github-actions | `devops/github-actions` | Workflow、CI/CD 实践 |
| docker | `devops/docker` | 镜像与容器 |
| linux-commands | `devops/linux-commands` | 常用 Linux/Shell 运维 |

### data

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| sql-optimization | `data/sql-optimization` | SQL 与索引优化 |
| data-visualization | `data/data-visualization` | 图表与可视化 |

### ai

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| prompt-engineering | `ai/prompt-engineering` | 提示词结构与技巧 |
| agent-design | `ai/agent-design` | Agent / 技能架构设计 |

### business

| 技能 | 路径 | 说明 |
| :--- | :--- | :--- |
| product-thinking | `business/product-thinking` | 产品思维与 MVP |
| project-planning | `business/project-planning` | 项目规划与排期 |

全部 **25** 个技能均含完整 Frontmatter（USE FOR / DO NOT USE）、When / Inputs / Workflow / Validation 等结构。C# 技能另对齐 [.NET Agent Skills](https://github.com/dotnet/skills) 的安全与验证约定（如改库需用户批准）。

C# 技能的结构与安全约定对齐微软官方 [.NET Agent Skills](https://github.com/dotnet/skills)（`USE FOR` / `DO NOT USE`、`Inputs`、`Workflow`、`Validation`、改库需用户批准等）。

## 相关导航

- [AGENTS.md](./AGENTS.md) — AI 路由规则与领域地图
- [docs/skill-triggers.md](./docs/skill-triggers.md) — 关键词映射表

## License

[MIT](./LICENSE)
