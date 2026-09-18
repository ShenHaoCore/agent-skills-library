---
name: sql-optimization
description: >
  Guides SQL query optimization: indexing strategies, reading query plans,
  reducing slow queries, and avoiding common SQL anti-patterns.
  USE FOR: speed up SQL; design or review indexes; interpret EXPLAIN / EXPLAIN ANALYZE;
  rewrite bad JOIN/filter/pagination; cut N+1-style repeated queries at SQL layer.
  关键词：SQL 优化、索引、慢查询、EXPLAIN、query plan、执行计划、复合索引、
  分页、选择性。
  DO NOT USE FOR: EF Core migration workflows (use programming/csharp/ef-core-migrations);
  chart/dashboard design (use data-visualization); Pandas wrangling
  (use programming/python/pandas-data); NoSQL schema design.
license: MIT
---

# SQL Optimization

定位慢查询并通过对索引、写法与执行计划的改进降低延迟与负载。

结果呈现与仪表盘交叉加载 `data/data-visualization`。

## When to Use

- 接口 / 报表 SQL 过慢
- 需要加索引或评估现有索引
- 解读 `EXPLAIN` / `EXPLAIN ANALYZE`
- 改写选择性低的过滤、坏 JOIN、不稳定分页
- 减少重复往返式查询（在 SQL/批处理层）

## When Not to Use

- EF Core 迁移 / `dotnet ef` → `programming/csharp/ef-core-migrations`
- 图表类型、仪表盘布局 → `data/data-visualization`
- Pandas DataFrame 清洗 / 分析细节 → `programming/python/pandas-data`
- NoSQL（Mongo 等）模式设计

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 数据库方言 | Yes | PostgreSQL / MySQL / SQL Server 等 |
| 慢查询文本 | Yes | 完整 SQL 或可复现片段 |
| 大致数据量 / 基数 | Recommended | 表行数、过滤选择性 |
| 执行计划 | Optional | `EXPLAIN` / `EXPLAIN ANALYZE` 输出 |
| 读写比例约束 | Optional | 避免过度索引拖累写入 |

## Workflow

### Step 1: 先测量

1. 记录耗时、返回行数、是否偶发。
2. 确认环境（生产只读副本 vs 可改索引的环境）。

### Step 2: 看计划

1. Seq Scan / Index Scan / 排序 / 哈希 / Nested Loop 是否合理。
2. 估计行数与实际行数是否严重偏离。

### Step 3: 索引与写法

1. 索引匹配过滤与连接键；复合索引注意列顺序。
2. 避免在列上包函数导致无法用索引（改写条件或用表达式索引，按方言）。
3. 只选需要的列；警惕 `SELECT *`。
4. 分页用稳定键；深翻页考虑 keyset pagination。

### Step 4: 对比与收敛

1. 改写后再比计划与耗时。
2. 防止过度索引拖累写入；删除无用重复索引。
3. 报表结果可视化需求交给 `data/data-visualization`。

## Examples

### 复合索引 + EXPLAIN（PostgreSQL 思路）

```sql
CREATE INDEX CONCURRENTLY ix_orders_customer_created
  ON orders (customer_id, created_at DESC);

EXPLAIN (ANALYZE, BUFFERS)
SELECT id, total
FROM orders
WHERE customer_id = 42
ORDER BY created_at DESC
LIMIT 20;
```

### 避免列上函数（概念）

```sql
-- BAD: 无法高效用 created_at 索引（视方言与索引而定）
-- WHERE DATE(created_at) = '2024-01-01'

-- BETTER: 范围条件
WHERE created_at >= TIMESTAMP '2024-01-01'
  AND created_at <  TIMESTAMP '2024-01-02'
```

## Validation

- [ ] 有测量基线（耗时/行数）再改
- [ ] 有计划依据（或明确无法取得计划的假设）
- [ ] 索引建议匹配过滤/排序/连接，并考虑写入成本
- [ ] 改写后对比计划或可复现基准
- [ ] 未把 ORM 迁移或图表设计塞进本技能范围

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 未看计划就加索引 | 先 EXPLAIN，再针对性建索引 |
| 过度索引 | 合并复合索引；评估写放大 |
| `SELECT *` + 宽表 | 只选需要列 |
| 列上函数导致索引失效 | 改写为 sargable 条件 |
| OFFSET 深分页 | keyset / seek 分页 |
| 优化完却不会讲结论 | 交叉 `data-visualization` 呈现 |

## References

- 相关技能：`data/data-visualization`
- EF 迁移（非本技能）：`programming/csharp/ef-core-migrations`
---
