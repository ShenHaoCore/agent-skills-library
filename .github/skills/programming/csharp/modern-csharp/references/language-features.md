# Modern C# Language Features (quick reference)

Prefer features supported by the project's `LangVersion`. Verify with `dotnet build`.

## C# 10+

| Feature | Typical use |
| :--- | :--- |
| File-scoped namespaces | 减少一层缩进 |
| Global using | 团队统一隐式 using（谨慎） |
| Record structs | 小组件不可变值 |
| Improved pattern matching | 属性模式、关系模式 |

## C# 11+

| Feature | Typical use |
| :--- | :--- |
| Raw string literals | 多行 SQL/JSON 文本 |
| Required members | 强制对象初始化完整 |
| Generic attributes / UTF-8 strings | 进阶场景按需 |

## C# 12+

| Feature | Typical use |
| :--- | :--- |
| Primary constructors | 简洁依赖捕获 |
| Collection expressions | `[1,2,3]`、扩展元素 |
| Alias any type | `using` 别名增强 |

## C# 13 注意

- 以目标 SDK 文档为准；升级 TFM 前不要假设可用
- 参考：[What's new in C#](https://learn.microsoft.com/dotnet/csharp/whats-new/)

## NRT quick rules

1. Enable project-wide nullable
2. Annotate public API first
3. Prefer `required` / constructors over `null!`
4. Suppressions (`!`, `#pragma`) need a one-line justification in review

## What not to modernize blindly

- Generated code (`*.g.cs`) — 改生成器输入
- Public serialized DTOs — 保持线契约；可用 `[JsonPropertyName]` 等保留线名
- EF 实体 — 遵循现有跟踪/导航约定，勿仅为「好看」改 record
