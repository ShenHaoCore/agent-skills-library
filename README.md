# agent-skills-library

**智能体技能库** — 通用型 AI Agent Skills 集合，适配 GitHub Copilot、Claude Code 等支持 [Agent Skills](https://agentskills.io) 的工具。

## 安装

```bash
npx skills add ShenHaoCore/agent-skills-library          # 交互选择
npx skills add ShenHaoCore/agent-skills-library -s '*' -y  # 全部安装
npx skills add ShenHaoCore/agent-skills-library -l         # 仅列表
npx skills add ShenHaoCore/agent-skills-library -s async-patterns -y  # 单个
```

也可用 `gh skill install ShenHaoCore/agent-skills-library`（隐藏目录时加 `--allow-hidden-dirs`）。

**对话安装：** 在 Cursor / Copilot Agent 等中说「把 ShenHaoCore/agent-skills-library 装到当前项目」，批准终端命令即可；装完建议新开对话。

**直接使用本仓：** `git clone` 后打开仓库，工具会扫描 `.github/skills/`。

## 结构

```text
.github/skills/<领域>/[<子领域>/]<技能名>/SKILL.md
```

| 文件 | 用途 |
| :--- | :--- |
| `AGENTS.md` | AI 领域路由 |
| `docs/skill-triggers.md` | 关键词 → 技能 |
| `.vscode/settings.json` | 可选 `chat.skillsLocations`（以 VS Code 版本为准） |

技能目录小写连字符；`name` 与目录名一致；`description` 含 What / When / 关键词 / NOT for。细节放 `references/`。

## 使用

用自然语言描述任务即可，例如「可取消的 C# 异步下载」「UI 走查」。也可让 AI 先读 `AGENTS.md`。

## 新增技能

1. 在 `.github/skills/.../<skill-name>/` 写 `SKILL.md`
2. 更新本清单与 `docs/skill-triggers.md`（可选：`AGENTS.md`）

## 技能清单（25）

| 领域 | 技能 |
| :--- | :--- |
| programming | `csharp/async-patterns` · `csharp/modern-csharp` · `csharp/naming-conventions` · `csharp/code-style` · `csharp/ef-core-migrations` · `python/pythonic-code` · `python/pandas-data` · `javascript/modern-js` |
| writing | `technical-writing` · `copywriting` · `editing` |
| productivity | `task-management` · `note-taking` · `time-blocking` |
| design | `ui-review` · `design-system` |
| devops | `github-actions` · `docker` · `linux-commands` |
| data | `sql-optimization` · `data-visualization` |
| ai | `prompt-engineering` · `agent-design` |
| business | `product-thinking` · `project-planning` |

路径均相对于 `.github/skills/`。C# 技能约定对齐 [dotnet/skills](https://github.com/dotnet/skills)。

## License

[MIT](./LICENSE)
