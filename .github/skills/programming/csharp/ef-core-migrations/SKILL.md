---
name: ef-core-migrations
description: >
  Guides Entity Framework Core schema migrations: create, review, apply, rollback,
  idempotent SQL scripts, and conflict recovery.
  USE FOR: dotnet ef migrations add/remove; database update to a target migration;
  production migrations script --idempotent; ModelSnapshot merge conflicts;
  reviewing Up/Down for data loss; Expand/Contract multi-step schema changes.
  关键词：EF Core 迁移、dotnet ef、migration、database update、回滚、生产 SQL、
  idempotent、ModelSnapshot、迁移冲突。
  DO NOT USE FOR: general SQL query tuning or EXPLAIN (use data/sql-optimization);
  EF Core query performance / N+1 / AsNoTracking (not this skill);
  Dapper-only or non-EF ORMs; inventing schema without a DbContext.
license: MIT
metadata:
  inspired-by: https://github.com/dotnet/skills
  microsoft-learn: https://learn.microsoft.com/ef/core/managing-schemas/migrations/
---

# EF Core Migrations

安全地创建、审查、应用与回滚 EF Core 迁移，并为生产生成可审查的幂等 SQL。  
**默认不执行会改库的命令**，除非用户明确批准。

## When to Use

- 实体/属性变更后需要 `migrations add`
- 本地或 CI 准备 `database update`
- 生产需要 `migrations script`（尤其 `--idempotent`）
- 处理迁移冲突、错误迁移、回滚 / `migrations remove`
- Expand/Contract（先扩后缩）分步发布

## When Not to Use

- 慢查询、N+1、`Include`、编译查询 → 不属于本技能（查询优化）
- 纯 SQL/索引调优且无 EF 迁移上下文 → `data/sql-optimization`
- 非 EF Core ORM
- 仅用 `EnsureCreated` 的玩具示例（应引导改为迁移）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| DbContext 项目路径 | Yes | 含 `DbContext` 的 `.csproj`（`--project`） |
| 启动项目路径 | Yes（多项目时） | 能解析配置/DI 的项目（`--startup-project`） |
| DbContext 类型名 | Optional | 多 Context 时用 `--context` |
| 目标环境 | Recommended | 开发本地 / 预发 / 生产（决定 update vs script） |

## Safety Gate

| 操作 | 代理默认行为 |
| --- | --- |
| `dotnet ef migrations add` | 可执行；生成后必须展示并审查 `Up`/`Down` |
| `dotnet ef migrations remove` | 仅当迁移**未**应用于共享/生产库且用户同意 |
| `dotnet ef database update` | **禁止擅自执行**；展示命令，等用户明确批准 |
| `dotnet ef migrations script` | 可生成文件供审查；由变更流程执行 SQL |

## Workflow

### Step 1: 基线

1. 解决方案可编译：`dotnet build`。
2. 确认 Design 包：`Microsoft.EntityFrameworkCore.Design`（常用 `PrivateAssets="all"`）。
3. 工具：优先 `dotnet tool restore`（本地 manifest）；否则再考虑全局 `dotnet-ef`。
4. 多项目始终带 `--project` 与 `--startup-project`。

### Step 2: 添加迁移

1. 命名用意图短语：`AddUserEmailIndex`、`RenameOrderStatus`。
2. 运行 `migrations add` 后立刻读生成的 `Up`/`Down` 与 `ModelSnapshot`。
3. Checkpoint：是否有丢数据、锁表风险、不可逆 Down、缺默认值的非空列。

### Step 3: 本地验证（需用户批准才 update）

1. 展示即将执行的 `database update` 命令与目标库连接含义。
2. 用户批准后更新；核对 `__EFMigrationsHistory`。
3. 跑与 schema 相关的测试。

### Step 4: 生产路径（脚本优先）

1. 生成幂等脚本：`migrations script --idempotent -o ...`。
2. 人工/变更单审查 SQL（索引、回填、锁）。
3. 预发执行 → 冒烟 → 生产执行同一脚本。
4. 代码与 schema 兼容：优先 Expand/Contract，避免一步删列。

### Step 5: 回滚与冲突

| 情况 | 动作 |
| --- | --- |
| 最后一次迁移未分享 | `migrations remove`（确认未应用到共享库） |
| 已应用需回退 | `database update <PreviousMigration>`（需批准）或正向修复迁移 |
| 多人同时加迁移 | 未推送：remove 后按主线重建；已生产：**禁止改历史 Up**，只加修复迁移 |
| Snapshot 冲突 | 以主分支意图为准合并，再 `build` 验证 |

## Examples

### 添加迁移（不自动 update）

```bash
dotnet ef migrations add AddUserEmailIndex \
  --project src/App.Infrastructure \
  --startup-project src/App.Api
```

### 移除最后一个未应用迁移

```bash
dotnet ef migrations remove \
  --project src/App.Infrastructure \
  --startup-project src/App.Api
```

### 回滚到指定迁移（需批准）

```bash
dotnet ef database update 20240101120000_AddUsers \
  --project src/App.Infrastructure \
  --startup-project src/App.Api
```

### 生产幂等 SQL

```bash
dotnet ef migrations script --idempotent -o ./artifacts/migrate.sql \
  --project src/App.Infrastructure \
  --startup-project src/App.Api

dotnet ef migrations script 20240101120000_AddUsers 20240201120000_AddOrders \
  --idempotent -o ./artifacts/migrate-range.sql \
  --project src/App.Infrastructure \
  --startup-project src/App.Api
```

### 迁移审查表

| 检查项 | 关注点 |
| --- | --- |
| 数据丢失 | `DropColumn` / 缩窄类型前是否有备份或双写 |
| 锁与时长 | 大表索引是否需在线/分步 |
| Down | 可回滚或明确标注不可逆 |
| 非空列 | default 或分步迁移 |

## Validation

- [ ] `dotnet build` 成功
- [ ] 新迁移的 `Up`/`Down` 已人工审查
- [ ] 未在未批准情况下执行 `database update`
- [ ] 生产变更使用已审查的 `--idempotent` 脚本（或等价变更流程）
- [ ] 无已部署迁移的历史 `Up` 被改写
- [ ] （批准后）`__EFMigrationsHistory` 与模型一致；相关测试通过

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 找不到 DbContext | `--context`、检查 Design 包与 startup 项目 |
| 已应用仍 `remove` | 先回退或新增反向/修复迁移 |
| 生产直接 `update` 失败难审计 | 改用幂等脚本 + 变更单 |
| `EnsureCreated` 代替迁移 | 删除该路径，改用 migrations |
| 单步改名+删旧列 | Expand → 回填 → Contract |
| 全局乱装 `dotnet-ef` 版本漂移 | 优先本地 tool manifest |

## References

- 生产与冲突：`references/production-migrations.md`
- 相关：`programming/csharp/async-patterns`（异步数据访问）
- [EF Core migrations](https://learn.microsoft.com/ef/core/managing-schemas/migrations/)
- [Apply migrations at runtime (caution)](https://learn.microsoft.com/ef/core/managing-schemas/migrations/applying)
- [Migration teams / conflicts](https://learn.microsoft.com/ef/core/managing-schemas/migrations/teams)
