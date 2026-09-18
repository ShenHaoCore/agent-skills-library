---
name: naming-conventions
description: >
  Applies C# naming conventions for types, members, parameters, locals,
  interfaces, async methods, private fields, and common .NET idioms.
  USE FOR: renaming symbols to match C#/.NET conventions; reviewing PascalCase
  vs camelCase; I-prefix interfaces; Async suffix; private field prefixes
  (_camelCase vs m_ vs none); boolean names; enum/event/delegate names;
  clarifying abbreviations and acronyms in identifiers.
  关键词：命名规范、naming conventions、PascalCase、camelCase、接口前缀、
  Async 后缀、私有字段、_camelCase、布尔命名、标识符。
  DO NOT USE FOR: EditorConfig / dotnet format / analyzer severity
  (use code-style); language feature modernization (use modern-csharp);
  async correctness beyond the Async suffix (use async-patterns);
  EF migration class names as the primary task (use ef-core-migrations).
license: MIT
metadata:
  inspired-by: https://github.com/dotnet/skills
  microsoft-learn: https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions
---

# C# Naming Conventions

按 .NET / C# 惯用命名命名标识符，使代码在团队与框架生态中可预期。  
格式化与分析器配置 → `code-style`；异步行为正确性 → `async-patterns`。

## When to Use

- 审查或统一类型、成员、参数、局部变量命名
- 接口是否加 `I`、异步方法是否加 `Async`
- 私有字段前缀策略（`_camelCase` 等）争议
- 布尔、事件、枚举、泛型参数命名不清
- 缩写 / 首字母缩略词大小写不一致

## When Not to Use

- 只要排版、`.editorconfig`、`dotnet format` → `code-style`
- 只要 record / NRT / 模式匹配等语法现代化 → `modern-csharp`
- 死锁、CancellationToken、ConfigureAwait → `async-patterns`
- 序列化线契约字段名（JSON 属性名）——保留线名，可用特性映射，勿为「好看」改破坏契约

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标代码 / 符号列表 | Yes | 要命名或审查的类型与成员 |
| 仓库既有约定 | Recommended | 现有字段前缀、接口风格；**有冲突时以仓库为准** |
| 公共 API 边界 | Recommended | 已发布符号重命名需兼容策略 |

## Workflow

### Step 1: 锁定仓库约定

1. 搜索现有私有字段、接口、异步方法样本（10+ 处）。
2. 若仓库已统一（例如全用 `_camelCase`），**跟随仓库**，不要强行换成另一派。
3. 无既有约定时，采用下方默认表（对齐 Microsoft 文档常见建议）。

### Step 2: 按种类套用规则

默认约定（无仓库冲突时）：

| 种类 | 风格 | 示例 |
| --- | --- | --- |
| 命名空间、类型（class/struct/record/enum/delegate） | PascalCase | `OrderService`, `UserId` |
| 接口 | PascalCase + `I` 前缀 | `IOrderRepository` |
| 公共/保护成员（方法、属性、事件） | PascalCase | `GetTotal`, `CreatedAt` |
| 异步方法 | PascalCase + `Async` 后缀 | `LoadAsync`, `SaveChangesAsync` |
| 参数、局部变量 | camelCase | `orderId`, `cancellationToken` |
| 私有字段 | `_` + camelCase | `_repository`, `_cts` |
| 常量 | PascalCase | `DefaultTimeout` |
| 静态只读「常量感」字段 | PascalCase | `DefaultOptions` |
| 类型参数 | 描述性或单字母 + 可选约束语义 | `T`, `TEntity`, `TResult` |
| 布尔 | 是/否语义前缀 | `IsActive`, `CanExecute`, `HasError` |
| 枚举类型与成员 | PascalCase；类型常用单数 | `OrderStatus.Pending` |
| 事件 | PascalCase 动词/短语 | `PropertyChanged`, `OrderSubmitted` |
| 抽象/基类后缀（可选惯用） | `Base` / 角色名 | `ControllerBase`（跟随框架） |

### Step 3: 特殊规则

1. **Async 后缀**：返回 awaitable（`Task`/`ValueTask`/`IAsyncEnumerable` 等）的方法加 `Async`；事件处理器例外（常无后缀）。
2. **不要**给同步方法加 `Async`；不要用 `Async` 表示「稍慢」的同步 API。
3. **接口**：对外契约用 `I`；避免 `I` 用于抽象类。
4. **缩写**：≥3 字母常按词大小写（`HtmlParser` 而非 `HTMLParser`）；常见两字母可全大写（`IOStream` 场景跟框架）。
5. **匈牙利命名 / `m_` / `s_`**：新代码避免；维护旧代码时局部一致即可。
6. **否定命名**：优先 `IsReady` 而非 `IsNotReady`；条件里用 `!`。
7. **公开 API 重命名**：需要 `[Obsolete]` 转发或视为破坏性变更，勿默默改名。

### Step 4: 应用与验证

1. 用语义重命名（IDE/LSP rename），禁止盲目文本替换。
2. 包含所有 `partial` 声明；不改生成代码，改生成器输入。
3. `dotnet build`；相关测试通过。

## Examples

```csharp
public interface IOrderRepository
{
    Task<Order?> GetAsync(string orderId, CancellationToken cancellationToken = default);
}

public sealed class OrderService(IOrderRepository repository, IClock clock)
{
    private readonly IOrderRepository _repository = repository;
    private static readonly TimeSpan DefaultTimeout = TimeSpan.FromSeconds(30);

    public async Task<bool> TrySubmitAsync(string orderId, CancellationToken cancellationToken)
    {
        var order = await _repository.GetAsync(orderId, cancellationToken);
        if (order is null || order.IsCancelled)
            return false;

        // ...
        return true;
    }
}

public enum OrderStatus
{
    Pending,
    Submitted,
    Cancelled
}
```

### 反例 → 正例

| 反例 | 正例 |
| --- | --- |
| `get_data()` | `GetData` / `GetDataAsync` |
| `IOrderRepositoryClass` | `IOrderRepository` + `OrderRepository` |
| `m_repo` / `this.repo` 混用 | 统一 `_repo`（或仓库既有风格） |
| `Flag1` / `bActive` | `IsActive` |
| `HTMLHelper` | `HtmlHelper`（跟团队/框架） |
| sync `LoadAsync()` 内无 awaitable | 去掉 `Async` 或改为真异步 |

## Validation

- [ ] 类型/公共成员 PascalCase；参数与局部 camelCase
- [ ] 接口 `I` 前缀；异步方法 `Async` 后缀（适用时）
- [ ] 私有字段与仓库约定一致
- [ ] 布尔名可读；枚举成员清晰
- [ ] 使用绑定感知重命名；`dotnet build` 成功
- [ ] 未破坏序列化/反射/公共契约线名（或已显式处理）

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 文本替换改到字符串/注释/无关符号 | IDE/LSP rename + 审查 diff |
| 为消歧给所有类加 `Class`/`Manager` 后缀 | 用角色清晰的名字，避免空洞后缀 |
| 私有字段三种前缀并存 | 选定一种并在本文件/本程序集收敛 |
| 改 JSON 属性名却未加映射 | 保留线名或加 `JsonPropertyName` |
| 给 `async void` 事件硬加 `Async` | 事件处理器保持框架惯用名 |

## References

- 速查表：`references/naming-cheatsheet.md`
- 相关：`programming/csharp/code-style`、`programming/csharp/async-patterns`、`programming/csharp/modern-csharp`
- [C# coding conventions](https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [Framework design guidelines — naming](https://learn.microsoft.com/dotnet/standard/design-guidelines/naming-guidelines)
