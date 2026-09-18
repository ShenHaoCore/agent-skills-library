---
name: pythonic-code
description: >
  Guides idiomatic Python (Pythonic) style: comprehensions, generators, context
  managers, type hints, EAFP, and common anti-patterns.
  USE FOR: cleaning or reviewing Python for idiomatic style; list/dict/set comps;
  generators; with / contextlib; progressive type hints; mutable default args;
  bare except; resource lifecycle; naming and flat structure.
  关键词：Python、优雅代码、Pythonic、列表推导、生成器、上下文管理器、类型提示、
  type hints、反模式、EAFP、snake_case。
  DO NOT USE FOR: Pandas DataFrame pipelines (use pandas-data); web framework
  tutorials (Django/Flask/FastAPI deep dives); non-Python languages;
  SQL optimization or visualization skills.
license: MIT
---

# Pythonic Code

把 Python 写得清晰、惯用、可维护：优先可读性与标准库习惯，避免「别的语言翻译腔」。表格数据分析见 `programming/python/pandas-data`。

## When to Use

- 重构「能跑但不 Pythonic」的代码
- 代码审查关注风格、推导式、生成器、`with`、类型标注
- 教学或统一团队 Python 习惯
- 消除可变默认参数、过度抽象、错误异常用法等反模式

## When Not to Use

- Pandas / DataFrame 清洗、groupby、merge → `pandas-data`
- 框架级路由、ORM、依赖注入教程（本技能只覆盖语言习惯）
- 非 Python 语言
- 纯 SQL / 可视化 → `data/sql-optimization`、`data/data-visualization`

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标代码或文件路径 | Recommended | 要编写/审查的 Python 模块或片段 |
| Python 版本 | Optional | 默认按 3.10+；影响 `X \| None`、`list[str]` 等语法 |
| 约束 | Optional | 是否允许改 API、是否必须保持兼容旧类型注解 |

## Workflow

### Step 1: 明确目标与约束

1. 区分「风格重构」与「行为变更」；默认不改对外行为。
2. 确认目标版本（3.10+ 可用内置泛型与 `|` 联合类型）。
3. 公共 API 优先补类型提示；内部可渐进标注。

### Step 2: 可读性优先

1. 推导式保持短小；复杂逻辑用普通循环或生成器函数。
2. 「扁平优于嵌套」：早返回、拆小函数，避免深层 if/for。
3. 命名用 `snake_case`；模块职责单一、体积可控。

### Step 3: 推导式与生成器

| 场景 | 做法 |
| --- | --- |
| 需要物化列表 | list comprehension |
| 大数据流 / 懒求值 | generator 表达式或 `yield` |
| 去重保序 | `dict.fromkeys(...)` 或显式循环 |
| 有副作用（IO、打印） | **不要**塞进推导式 |

### Step 4: 资源与上下文管理器

1. 文件、锁、连接、临时目录一律 `with`。
2. 自定义资源：实现 `__enter__`/`__exit__` 或用 `contextlib.contextmanager`。
3. 禁止依赖手动 `close()` 作为唯一释放路径。

### Step 5: 类型提示与异常（EAFP）

1. 公共函数标注参数与返回值；用 `list[str]`、`X | None`（3.10+）或 `Optional`（旧版）。
2. 捕获**具体**异常；禁止裸 `except:` / 笼统 `except Exception` 后静默吞掉。
3. EAFP 适合「预期可能失败」的边界；高频正常路径不要用异常当控制流。

### Step 6: 扫反模式并验证

1. 对照 `references/anti-patterns.md` 与 Validation 清单。
2. 有测试时跑相关用例；无测试时至少做冒烟导入 / 关键路径手测。

## Examples

### 推导式 vs 循环

```python
# Prefer
names = [u.name.strip() for u in users if u.active]

# Generator for large streams
ids = (row["id"] for row in cursor if row["id"] is not None)
```

### 上下文管理器

```python
from pathlib import Path

path = Path("data.txt")
with path.open("r", encoding="utf-8") as f:
    for line in f:
        process(line)
```

### 类型提示

```python
def find_user(users: list[dict[str, str]], user_id: str) -> dict[str, str] | None:
    for user in users:
        if user.get("id") == user_id:
            return user
    return None
```

### 反模式：可变默认参数

```python
# BAD
def append_item(item: str, bucket: list[str] = []) -> list[str]:
    bucket.append(item)
    return bucket

# GOOD
def append_item(item: str, bucket: list[str] | None = None) -> list[str]:
    if bucket is None:
        bucket = []
    bucket.append(item)
    return bucket
```

### 反模式：手动管理本可用 with 的资源

```python
# BAD
f = open("x.txt")
data = f.read()
f.close()

# GOOD
with open("x.txt", encoding="utf-8") as f:
    data = f.read()
```

## Validation

- [ ] 无可变默认参数（`list`/`dict`/`set` 作默认值）
- [ ] 文件/连接/锁使用 `with` 或等价上下文管理
- [ ] 推导式不过度嵌套（一般 ≤ 2 层逻辑；无副作用）
- [ ] 公共函数有类型提示（适用时）
- [ ] 异常类型具体且带有用信息；无裸 `except:`
- [ ] 命名 `snake_case`；无明显「翻译腔」结构
- [ ] （可选）相关测试或冒烟检查通过

```bash
# 粗查可疑可变默认与裸 except（需人工确认）
rg -n --glob '*.py' 'def .*=\s*\[\]|def .*=\s*\{\}|except\s*:'
```

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 可变默认参数共享状态 | `None` + 函数内初始化 |
| 推导式嵌套过深或含副作用 | 改为循环 / 生成器函数 |
| 手动 `open`/`close` | `with` / `Path.open` |
| 裸 `except:` 吞中断 | 捕获具体异常并记录/上抛 |
| `type(x) == list` | 多数情况用 `isinstance` |
| 循环内字符串 `+=` | `"".join` 或列表收集后 join |
| 为「看起来 OOP」过度封装 | 函数 + 模块优先；类表达有状态的概念 |

## References

- 反模式清单：`references/anti-patterns.md`
- 相关技能：`programming/python/pandas-data`
- [PEP 8 — Style Guide](https://peps.python.org/pep-0008/)
- [PEP 484 / typing](https://docs.python.org/3/library/typing.html)
- [contextlib](https://docs.python.org/3/library/contextlib.html)
