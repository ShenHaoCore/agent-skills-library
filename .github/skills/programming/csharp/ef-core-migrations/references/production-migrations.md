# Production EF Core Migrations

## Idempotent scripts

`--idempotent` 在脚本中检查 `__EFMigrationsHistory`，使部分已应用环境更安全。仍须在预发验证。

## Recommended production flow

1. CI 构建并生成 `migrations script --idempotent`
2. 人工 / 自动审查 SQL（锁、数据迁移、回填）
3. 预发执行并冒烟
4. 生产执行同一脚本（或经审批的变更单）
5. 应用滚动升级：代码与 schema 兼容（Expand → Contract）

## Agent safety (dotnet/skills-aligned)

- **可**生成迁移与 SQL 文件并展示 diff
- **不可**在未获用户明确批准时对共享/生产库执行 `database update`
- 优先本地 `dotnet tool restore`，避免全局工具版本漂移

## Expand / contract

- **Expand**：加可空列、新表、双写
- **Migrate data**：后台回填
- **Contract**：确认无流量后删旧列 / 旧路径

避免单次迁移同时「改名 + 改语义 + 删旧列」。

## Conflict playbook

1. 确认本地迁移是否已推送 / 已应用于共享库
2. 未推送：`migrations remove`，拉主线后重建
3. 已推送未生产：团队协调后再替换（需一致同意）
4. 已生产：只加修复迁移，不改历史 `Up`

## Rollback strategy

| Situation | Action |
| :--- | :--- |
| 仅本地 | 批准后 `database update Prev` + `migrations remove` |
| 已生产且 Down 安全 | 执行 Down / update 到上一迁移（变更流程） |
| Down 不安全 | 正向修复迁移 + 数据修复脚本 |

## Tooling

```bash
# Prefer local manifest
dotnet tool restore
dotnet ef migrations add <Name> --project <Infra> --startup-project <Host>
dotnet ef migrations script --idempotent -o ./artifacts/migrate.sql \
  --project <Infra> --startup-project <Host>
```
