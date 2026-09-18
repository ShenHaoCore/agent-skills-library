# AGENTS.md — 智能体技能库顶层导航

本文件供 AI Agent 快速了解仓库结构并选择合适技能。人类协作者也可将其作为协作指南。

> **注意**：`AGENTS.md` 不会被自动加载。需要时请主动读取本文件，或结合 `docs/skill-triggers.md` 做关键词路由。

---

## 仓库定位

- **名称**：agent-skills-library（智能体技能库）
- **性质**：通用型、多领域、可扩展的 Agent Skills 集合
- **不绑定**：特定编程语言、技术栈或行业
- **技能根目录**：`.github/skills/`（GitHub Copilot 默认识别路径）

---

## 领域包列表

| 领域包 | 路径 | 用途 |
| :--- | :--- | :--- |
| programming | `.github/skills/programming/` | 编程语言、框架、代码风格与工程实践 |
| writing | `.github/skills/writing/` | 技术文档、文案、编辑与改写 |
| productivity | `.github/skills/productivity/` | 任务管理、笔记、时间规划 |
| design | `.github/skills/design/` | UI 评审、设计系统与视觉一致性 |
| devops | `.github/skills/devops/` | CI/CD、容器、Linux 运维 |
| data | `.github/skills/data/` | SQL、数据分析与可视化 |
| ai | `.github/skills/ai/` | 提示工程、Agent 设计 |
| business | `.github/skills/business/` | 产品思维、项目规划 |

---

## 目录约定

```text
.github/skills/<domain>/[<subdomain>/]<skill-name>/SKILL.md
```

- **第一层**：领域包（如 `programming`、`writing`）
- **第二层**：子领域或技能组（如 `csharp`、`python`）；若不需要细分，可直接放技能
- **第三层**：具体技能目录（小写 + 连字符），内含必需的 `SKILL.md`
- **可选**：`references/`、`scripts/`、`assets/`

---

## AI 选择技能的路由规则

1. **先定领域，再定技能**  
   根据用户意图匹配领域包，再在领域内按关键词匹配技能 `description`。

2. **优先精确匹配**  
   用户明确提到技术/场景（如「EF Core 迁移」「GTD」「UI 走查」）时，加载对应单一技能，不要一次加载整个领域。

3. **关键词歧义时查触发表**  
   读取 `docs/skill-triggers.md`，按关键词表选择推荐技能。

4. **单技能优先**  
   每个技能只覆盖一类明确任务。除非任务明显跨领域，否则一次只加载 1 个主技能。

5. **需要细节再读 references**  
   `SKILL.md` 给流程与要点；详细清单、长示例放在 `references/`，按需读取。

6. **可交叉引用，勿重复加载**  
   技能正文若引用了其他技能，仅在当前技能不足时再加载被引用技能。

---

## 推荐加载顺序

```text
1. 本文件 AGENTS.md（可选，建立全局地图）
2. docs/skill-triggers.md（关键词命中时）
3. 目标技能的 SKILL.md（必需）
4. 该技能 references/ 下相关文档（按需）
5. 被引用的兄弟技能 SKILL.md（仅当任务需要）
```

---

## 路由示例

| 用户意图 | 优先加载 |
| :--- | :--- |
| C# 异步、async/await、ConfigureAwait、取消令牌、ValueTask、Channels | `programming/csharp/async-patterns` |
| EF Core 迁移、dotnet ef、回滚、生产 SQL、ModelSnapshot 冲突 | `programming/csharp/ef-core-migrations` |
| 现代 C# 语法、record、NRT、pattern matching、集合表达式 | `programming/csharp/modern-csharp` |
| C# 命名规范、PascalCase、接口 I 前缀、Async 后缀、私有字段 | `programming/csharp/naming-conventions` |
| EditorConfig、dotnet format、代码风格、分析器严重级别 | `programming/csharp/code-style` |
| Python 优雅写法、列表推导、类型提示 | `programming/python/pythonic-code` |
| Pandas、DataFrame、数据清洗 | `programming/python/pandas-data` |
| 现代 JavaScript、ES 新特性 | `programming/javascript/modern-js` |
| 技术写作、API 文档、README | `writing/technical-writing` |
| 营销文案、落地页文案 | `writing/copywriting` |
| 润色、校对、改写 | `writing/editing` |
| 任务管理、GTD、看板、优先级 | `productivity/task-management` |
| 笔记方法、知识整理 | `productivity/note-taking` |
| 时间块、日程规划 | `productivity/time-blocking` |
| UI 评审、设计走查、可用性 | `design/ui-review` |
| 设计系统、组件规范、token | `design/design-system` |
| GitHub Actions、CI/CD workflow | `devops/github-actions` |
| Docker、镜像、容器 | `devops/docker` |
| Linux 命令、shell 运维 | `devops/linux-commands` |
| SQL 优化、索引、慢查询 | `data/sql-optimization` |
| 图表、可视化、dashboard | `data/data-visualization` |
| 提示词、prompt、少样本 | `ai/prompt-engineering` |
| Agent 架构、工具调用、技能设计 | `ai/agent-design` |
| 产品思维、需求、MVP | `business/product-thinking` |
| 项目规划、里程碑、排期 | `business/project-planning` |

---

## 新增技能时 AI 应遵守的约定

1. 目录名：小写字母 + 连字符，最长 64 字符
2. `name` 必须与技能目录名完全一致
3. `description` 必须包含 What / When / Keywords（中英）/ NOT for
4. 正文建议不超过 500 行；细节放入 `references/`
5. 更新 `README.md` 技能清单与 `docs/skill-triggers.md` 映射表
6. 不创建与现有技能高度重复的技能

---

## 相关文件

- 人类说明：`README.md`
- 关键词映射：`docs/skill-triggers.md`
- 技能根目录：`.github/skills/`
