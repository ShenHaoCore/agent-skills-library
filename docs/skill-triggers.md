# Skill Triggers — 关键词到技能映射

本文件是 Agent 的补充导航：当用户表述模糊或关键词跨领域时，用下表快速定位技能。

路径均相对于 `.github/skills/`。

---

## 映射表

| 关键词 | 推荐技能 |
| :--- | :--- |
| 异步, async, await, Task, ConfigureAwait, ValueTask, CancellationToken | programming/csharp/async-patterns |
| EF Core, 迁移, migration, database update, 回滚迁移, dotnet ef | programming/csharp/ef-core-migrations |
| 现代 C#, record, pattern matching, nullable, NRT, C# 12, C# 13, primary constructor, collection expressions, required | programming/csharp/modern-csharp |
| 命名规范, naming, PascalCase, camelCase, 接口前缀, Async 后缀, 私有字段, _camelCase | programming/csharp/naming-conventions |
| EditorConfig, dotnet format, 代码风格, code style, EnforceCodeStyleInBuild, IDE0055, 分析器 | programming/csharp/code-style |
| Python, 优雅代码, Pythonic, 列表推导, 生成器, 类型提示 | programming/python/pythonic-code |
| Pandas, DataFrame, 数据清洗, 表格分析, groupby | programming/python/pandas-data |
| JavaScript, 现代 JS, ES modules, optional chaining, async JS | programming/javascript/modern-js |
| 技术写作, 文档, documentation, API 文档, README 写作 | writing/technical-writing |
| 文案, copywriting, 营销文案, 落地页, slogan | writing/copywriting |
| 编辑, 润色, 校对, editing, 改写, proofreading | writing/editing |
| 任务管理, GTD, todo, 看板, 优先级, inbox | productivity/task-management |
| 笔记, note-taking, Zettelkasten, 知识管理, PKM | productivity/note-taking |
| 时间块, time-blocking, 日程, 专注时段, calendar | productivity/time-blocking |
| UI 评审, 设计走查, 可用性, UX review, spacing | design/ui-review |
| 设计系统, design system, token, 组件库规范 | design/design-system |
| GitHub Actions, CI, CD, workflow, yaml pipeline | devops/github-actions |
| Docker, Dockerfile, 镜像, 容器, compose | devops/docker |
| Linux, shell, bash, 命令行, 运维命令 | devops/linux-commands |
| SQL 优化, 索引, 慢查询, EXPLAIN, query plan | data/sql-optimization |
| 数据可视化, chart, dashboard, 图表, visualization | data/data-visualization |
| 提示词, prompt, LLM, 少样本, chain-of-thought | ai/prompt-engineering |
| Agent 设计, 工具调用, skill 设计, multi-agent | ai/agent-design |
| 产品思维, MVP, 需求, user story, 产品决策 | business/product-thinking |
| 项目规划, 里程碑, 排期, roadmap, WBS | business/project-planning |
| 死锁, deadlock, ConfigureAwait(false), sync-over-async | programming/csharp/async-patterns |
| 生产迁移, idempotent SQL, migrations script | programming/csharp/ef-core-migrations |
| 反模式, anti-pattern, with 语句, context manager | programming/python/pythonic-code |
| 面向读者, audience, 操作步骤, how-to | writing/technical-writing |
| 每日回顾, weekly review, Eisenhower | productivity/task-management |
| 无障碍, a11y, contrast, ARIA, 可访问性 | design/ui-review |
| 矩阵构建, matrix, cache, secrets, OIDC | devops/github-actions |
| 角色设定, system prompt, 输出格式, JSON schema | ai/prompt-engineering |
| 价值主张, value proposition, 优先级排序 | business/product-thinking |
| 风险缓冲, critical path, sprint planning | business/project-planning |

---

## 使用建议

1. **精确优先**：表中多关键词命中同一技能时，直接加载该技能。
2. **多技能命中**：若命中 2 个以上不同技能，选与用户主目标最接近的一个；其余仅作交叉引用。
3. **未命中**：回退到 `AGENTS.md` 领域包列表，先定领域再浏览该领域下技能的 `description`。
4. **新增映射**：新增技能后，至少补充 1–3 条高频关键词到本表。

---

## 维护约定

- 关键词同时包含中英文，提高语义匹配命中率
- 一行一个主技能；不要把互斥技能写在同一行
- 定期清理已删除技能的映射
