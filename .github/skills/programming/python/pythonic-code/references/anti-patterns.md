# Python Anti-Patterns

配合 `../SKILL.md` 的 Workflow Step 6 与 Validation 使用。本文件列高频反模式与简短正例；不重复完整技能流程。

## High-frequency issues

| Anti-pattern | Why | Prefer |
| :--- | :--- | :--- |
| Mutable default args | 跨调用共享状态 | `None` + 内部初始化 |
| Bare `except:` | 吞掉 KeyboardInterrupt 等 | 具体异常 |
| `from module import *` | 污染命名空间 | 显式导入 |
| Comparing with `== True` | 冗余、易错 | 直接真值测试 |
| Using `type(x) == list` | 拒绝子类 | `isinstance(x, list)` |
| String concat in loop | 性能差 | `"".join` / 列表收集 |
| Deep nesting | 难读 | 早返回、拆函数 |
| Manual `open`/`close` only | 异常路径易泄漏 | `with` / `Path.open` |

## Comprehensions

**Good**: `[normalize(x) for x in items if x]`

**Too clever**: 嵌套多层 + 副作用（打印、写文件）——改用循环或生成器函数。

## Generators

```python
def read_nonzero(paths: list[str]):
    for path in paths:
        with open(path, encoding="utf-8") as f:
            for line in f:
                line = line.strip()
                if line:
                    yield line
```

## contextlib helpers

```python
from contextlib import contextmanager

@contextmanager
def timed(label: str):
    import time
    start = time.perf_counter()
    try:
        yield
    finally:
        print(f"{label}: {time.perf_counter() - start:.3f}s")
```

## Quick links back to skill

- Validation checklist → `../SKILL.md`「Validation」
- Pandas 表格流程 → `../../pandas-data/SKILL.md`
