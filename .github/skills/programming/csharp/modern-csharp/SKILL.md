---
name: modern-csharp
description: >
  Modernizes C# code with current language idioms: records, pattern matching,
  nullable reference types (NRT), primary constructors, collection expressions,
  required members, and file-scoped namespaces.
  USE FOR: upgrading style to C# 10–13; introducing records/DTOs; enabling or
  fixing nullable warnings; switch expressions and list patterns; collection
  expressions; primary constructors; required/init properties.
  关键词：现代 C#、record、pattern matching、nullable、NRT、primary constructor、
  collection expressions、required、C# 12、C# 13、文件范围命名空间。
  DO NOT USE FOR: async/await deep dives (use async-patterns); EF Core migrations
  (use ef-core-migrations); framework TFM upgrades as the primary task
  (retarget first, then apply language features); non-C# languages.
license: MIT
metadata:
  inspired-by: https://github.com/dotnet/skills
  microsoft-learn: https://learn.microsoft.com/dotnet/csharp/whats-new/
---

# Modern C#

在**不改变可观察行为**的前提下，用当代 C# 惯用法提升表达力与空安全。  
异步专题 → `async-patterns`；数据库迁移 → `ef-core-migrations`。

## When to Use

- 旧式 DTO/样板改为 `record`、主构造函数、`required`/`init`
- 启用或清理可空引用类型（NRT）警告
- 用 pattern matching / switch 表达式替换深层 if-else
- 采用集合表达式、文件范围命名空间、全局 using（团队约定下）
- 代码审查要求「更现代的 C#」

## When Not to Use

- 任务本质是异步正确性 / 死锁 → `async-patterns`
- 任务本质是 EF 迁移 → `ef-core-migrations`
- 需要改公共 API 空契约且属于破坏性变更 → 先与调用方约定，再改注解
- 仅格式化（`dotnet format`）而无语言特性目标

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标项目/文件 | Yes | `.csproj` 或要现代化的 `.cs` |
| `TargetFramework` / `LangVersion` | Recommended | 决定可用特性 |
| 是否已启用 NRT | Recommended | `<Nullable>enable</Nullable>` |

## Workflow

### Step 1: 确认语言版本

1. 读取 `TargetFramework` 与 `LangVersion`（或 SDK 默认）。
2. 只推荐目标 TFM 真实支持的特性（建议 .NET 8+ / C# 12+）。
3. Checkpoint：不要建议当前编译器无法解析的语法。

### Step 2: 选择安全的现代化操作（一次一类）

| 操作 | 做 | 不做 |
| --- | --- | --- |
| DTO 不可变 | `record` / `record struct` | 把有身份可变实体强行改 record 却保留可变字段语义混乱 |
| 空安全 | 启用 NRT，用 `?`、模式、`required` 修复 | 盲目 `!` 镇压警告 |
| 分支 | switch 表达式、属性/列表模式 | 牺牲可读性的超长表达式 |
| 集合 | 集合表达式（版本允许时） | 在必须用特定集合类型 API 时硬换 |
| 构造 | 主构造函数捕获真正需要的依赖 | 把无关字段塞进主构造「图省事」 |

### Step 3: 可空引用类型（NRT）要点

1. 项目级 `<Nullable>enable</Nullable>`（或逐文件 `#nullable enable`）。
2. 意图表达：可空 → `T?`；不可空且必须由调用方提供 → `required`。
3. 修复顺序：公共 API 签名 → 字段/属性初始化 → 局部变量。
4. `!`（null-forgiving）仅在有证据保证非空时使用，并倾向于收敛而非扩散。
5. 序列化/ORM 实体：按框架约定初始化（如 EF 可导航属性策略），不要为消警告破坏映射。

### Step 4: 应用编辑并验证

1. 小步修改；行为应保持不变（纯结构/表达现代化）。
2. `dotnet build`：0 error；NRT 警告只减不增（除非刻意收紧并已沟通）。
3. 跑相关单元测试。

## Examples

### record + 模式匹配

```csharp
public sealed record UserDto(string Id, string Email);

string Describe(object shape) => shape switch
{
    Circle { Radius: > 0 } c => $"circle r={c.Radius}",
    Rectangle { Width: var w, Height: var h } => $"rect {w}x{h}",
    _ => "unknown"
};
```

### required / init

```csharp
public sealed class CreateOrderRequest
{
    public required string CustomerId { get; init; }
    public required IReadOnlyList<string> SkuIds { get; init; }
    public string? Note { get; init; }
}
```

### 集合表达式（C# 12+）

```csharp
int[] ints = [1, 2, 3];
List<string> tags = ["a", "b", ..other];
```

### 主构造函数（简洁 DI 捕获）

```csharp
public sealed class OrderService(IOrderRepository repo, IClock clock)
{
    public Task<Order> GetAsync(string id, CancellationToken ct) =>
        repo.GetAsync(id, ct);
}
```

## Validation

- [ ] 所用语法符合项目 `LangVersion` / TFM
- [ ] `dotnet build` 成功
- [ ] NRT 警告未无故增加；无大面积无证据的 `!`
- [ ] 可观察行为未变（测试通过或已说明无测试覆盖的风险）
- [ ] 未把异步/迁移问题塞进本次现代化

```bash
dotnet build
dotnet test --filter FullyQualifiedName~<RelevantTests>
```

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 在旧 `LangVersion` 上建议 C# 12 语法 | 先确认或提升语言版本 |
| 用 `!` 批量消警告 | 标注真实空性或重构初始化 |
| record 用于需要引用身份/可变跟踪的实体 | DTO 用 record；领域实体按现有模式 |
| 主构造函数参数变成无意义公开成员 | 只捕获需要的依赖；注意 `class` vs `record` 差异 |
| 一次混入行为变更 | 拆分 PR/步骤：先结构，后行为 |

## References

- 特性速查：`references/language-features.md`
- 相关：`programming/csharp/async-patterns`、`programming/csharp/ef-core-migrations`、`programming/csharp/naming-conventions`、`programming/csharp/code-style`
- [What's new in C#](https://learn.microsoft.com/dotnet/csharp/whats-new/)
- [Nullable reference types](https://learn.microsoft.com/dotnet/csharp/nullable-references)
- [Records](https://learn.microsoft.com/dotnet/csharp/language-reference/builtin-types/record)
- [Pattern matching](https://learn.microsoft.com/dotnet/csharp/fundamentals/functional/pattern-matching)
