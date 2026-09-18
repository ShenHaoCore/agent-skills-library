---
name: pandas-data
description: >
  Guides Pandas tabular analysis: DataFrame wrangling, cleaning, groupby,
  merges, and reproducible export.
  USE FOR: reading/writing CSV Excel Parquet; missing values and dtypes;
  groupby/agg/pivot; merge/concat; exploratory describe/value_counts;
  avoiding chained-assignment pitfalls.
  关键词：Pandas、DataFrame、数据清洗、groupby、merge、表格分析、CSV、Parquet。
  DO NOT USE FOR: general Pythonic style without Pandas (use pythonic-code);
  SQL index/EXPLAIN tuning (use sql-optimization); chart design
  (use data-visualization); non-Python stacks.
license: MIT
---

# Pandas Data

用 Pandas 完成表格数据的读取、清洗、聚合与导出，保持可复现的分析步骤。  
通用 Python 风格见 `programming/python/pythonic-code`。

## When to Use

- 读写 CSV / Excel / Parquet
- 缺测值、类型转换、去重、过滤
- `groupby` / pivot / merge / concat
- 快速探索性分析（`describe`、`value_counts`）

## When Not to Use

- 无 DataFrame、只要 Pythonic 风格 → `pythonic-code`
- 慢 SQL / 索引 / 执行计划 → `sql-optimization`
- 选图与仪表盘叙事 → `data-visualization`

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 数据路径或已有 DataFrame | Yes | 输入来源 |
| 预期输出 | Yes | 汇总表、指标或导出文件 |
| Python / pandas 版本 | Recommended | 建议 3.10+ 与较新 pandas |

## Workflow

1. `read_*` 后检查 `shape`、`dtypes`、头部样本。
2. 清洗顺序：类型 → 缺测 → 去重 → 过滤异常。
3. 聚合用 `groupby(...).agg(...)`；merge 前核对键唯一性。
4. 避免链式赋值陷阱：分步赋值或显式 `.copy()`。
5. 大文件：列筛选、分块、或 Parquet。
6. 写出前校验行数与关键指标。

## Examples

```python
import pandas as pd

df = pd.read_csv("orders.csv", parse_dates=["created_at"])
df = df.dropna(subset=["user_id"]).drop_duplicates(subset=["order_id"])
summary = (
    df.groupby("country", as_index=False)
      .agg(revenue=("amount", "sum"), orders=("order_id", "count"))
      .sort_values("revenue", ascending=False)
)
```

## Validation

- [ ] dtypes 合理；关键列无意外全空
- [ ] 聚合键与行数符合业务预期
- [ ] 无沉默的 SettingWithCopy 风险（或已消除）
- [ ] 导出文件可再读回并对齐校验列

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| `ToList`/过早物化前过滤 | 保持向量化过滤后再导出 |
| merge 笛卡尔爆炸 | 检查键唯一性与 how |
| 解析日期失败变 object | `parse_dates` / `to_datetime(errors=...)` |
| 大 CSV 一次读爆内存 | usecols、chunksize、Parquet |

## References

- 风格：`programming/python/pythonic-code`
- 可视化：`data/data-visualization`
- SQL：`data/sql-optimization`
