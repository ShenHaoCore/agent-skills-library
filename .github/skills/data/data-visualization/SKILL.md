---
name: data-visualization
description: >
  Guides choosing and designing charts for clear data storytelling: chart types,
  visual encoding, labels, and dashboard layout basics.
  USE FOR: pick a chart for metrics; critique or redesign a dashboard; encode
  trend/comparison/distribution/composition; titles, units, legends, color semantics.
  关键词：数据可视化、chart、dashboard、图表、visualization、坐标轴、编码、
  折线图、条形图、仪表盘。
  DO NOT USE FOR: SQL tuning / EXPLAIN (use sql-optimization);
  Pandas wrangling details (use programming/python/pandas-data);
  UI component design systems (use design/design-system).
license: MIT
---

# Data Visualization

选对图表与编码，让读者在几秒内读懂结论。

慢查询与取数优化交叉加载 `data/sql-optimization`；复杂清洗可参考 `programming/python/pandas-data`。

## When to Use

- 为指标选图（趋势、对比、分布、构成）
- 改善仪表盘拥挤、误导性坐标
- 规定标题、单位、图例与颜色语义
- 评审图表是否支持既定结论

## When Not to Use

- SQL 索引 / 执行计划调优 → `data/sql-optimization`
- Pandas DataFrame 清洗细节 → `programming/python/pandas-data`
- 设计系统 token / 组件库 → `design/design-system`
- 无数据故事、纯装饰性插画

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 要回答的问题 | Yes | 比较？趋势？构成？关系？ |
| 数据表或汇总结果 | Recommended | 干净的字段与粒度 |
| 受众 | Optional | 执行层 KPI vs 分析下钻 |
| 媒介 | Optional | 单图、报表、实时仪表盘 |

## Workflow

### Step 1: 先结论后图形

1. 用一句话写出结论或假设。
2. 选能直接支持该结论的图，而非先堆图表。

### Step 2: 映射图表类型

| 意图 | 优先图形 |
| --- | --- |
| 趋势 | 折线 |
| 比较 | 条形（类别多时水平条） |
| 构成 | 堆叠条 / 树图；分类过多慎用饼图 |
| 分布 | 直方图 / 箱线 |
| 关系 | 散点 |

### Step 3: 编码与标注

1. 坐标从合适基线开始；标注单位与时间范围。
2. 颜色类别可控；色盲友好；勿用面积误导。
3. 标题陈述发现，而非只重复轴名。

### Step 4: 仪表盘布局

1. 先 KPI，后下钻；一屏一个主问题。
2. 避免无关联小图墙。
3. 取数过慢时回到 `data/sql-optimization`。

## Examples

### 渠道收入对比

```text
问题：各渠道本周收入对比
图：水平条形图，按收入降序
标题：本周收入 by 渠道（USD）
注释：标注 Top1 与中位数
```

### 趋势图检查点

```text
问题：近 12 周活跃用户是否回升？
图：折线 + 清晰周粒度
避免：双 Y 轴无说明；截断 Y 轴夸大波动
```

## Validation

- [ ] 图直接支持那句结论
- [ ] 类型匹配意图（趋势/比较/构成/分布）
- [ ] 单位、时间范围、图例齐全
- [ ] 颜色类别不过载；非仅靠颜色传达关键状态
- [ ] 仪表盘主问题唯一；拥挤图已删减
- [ ] 取数/性能问题已分流到 `sql-optimization`（若存在）

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 先选花哨图再找故事 | 先一句话结论 |
| 饼图分类过多 | 条形或合并「其他」 |
| 截断坐标轴误导 | 标明基线；谨慎截断 |
| 仪表盘无主问题 | 一屏一问；KPI 置顶 |
| 把慢 SQL 当可视化问题 | 交叉 `sql-optimization` |

## References

- 相关技能：`data/sql-optimization`
- 数据清洗（按需）：`programming/python/pandas-data`
---
