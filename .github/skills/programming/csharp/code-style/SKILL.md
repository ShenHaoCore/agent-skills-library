---
name: code-style
description: >
  Configures and applies C# code style via .editorconfig, IDE/analyzer rules,
  dotnet format, and build-enforced style settings.
  USE FOR: adding or tuning .editorconfig; enabling EnforceCodeStyleInBuild;
  running dotnet format; aligning indentation, var preferences, namespace style,
  using-directive order; setting analyzer severities (IDE*/CA*); Directory.Build
  style defaults; fixing style-only CI failures.
  关键词：代码风格、EditorConfig、dotnet format、分析器、IDE000x、CA 规则、
  EnforceCodeStyleInBuild、缩进、var、格式化。
  DO NOT USE FOR: identifier naming rules as the primary task (use
  naming-conventions); C# language feature modernization (use modern-csharp);
  async correctness (use async-patterns); EF migrations (use ef-core-migrations);
  MSBuild project modernization as the main goal (Directory.Build only when
  wiring style properties).
license: MIT
metadata:
  inspired-by: https://github.com/dotnet/skills
  microsoft-learn: https://learn.microsoft.com/dotnet/fundamentals/code-analysis/code-style-rule-options
---

# C# Code Style

用 `.editorconfig` + 分析器 + `dotnet format` 统一格式与风格偏好，并在构建中可强制执行。  
标识符如何命名 → `naming-conventions`；语法现代化 → `modern-csharp`。

## When to Use

- 新建或调整 `.editorconfig` / 根目录风格配置
- CI 因格式或 IDE 风格规则失败
- 启用 `EnforceCodeStyleInBuild`、`TreatWarningsAsErrors`（风格子集）
- 统一 `var`、`this.`、文件范围命名空间、using 排序、缩进
- 设定 `dotnet_diagnostic.IDExxxx.severity` / `CA` 严重级别

## When Not to Use

- 主要问题是符号命名（PascalCase/`Async`/字段前缀）→ `naming-conventions`
- 主要问题是引入 record/NRT 等语言特性 → `modern-csharp`
- 行为/逻辑 bug、异步死锁 → 对应功能技能
- 大规模无审查的全仓自动 format（先小范围试点）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 仓库根路径 | Yes | 放置 `.editorconfig` 的位置 |
| 现有 `.editorconfig` | Recommended | 有则增量修改，避免推倒重来 |
| 是否在 CI/编译强制 | Recommended | 决定 severity 与 `EnforceCodeStyleInBuild` |
| 目标项目 | Optional | 单项目试点 vs 全解决方案 |

## Workflow

### Step 1: 发现现状

1. 查找已有 `.editorconfig`、`.globalconfig`、`Directory.Build.props`。
2. 查看 CI 是否已跑 `dotnet format --verify-no-changes`。
3. Checkpoint：记录当前是否已 `EnforceCodeStyleInBuild`。

### Step 2: 建立或扩展 `.editorconfig`

1. 根目录添加/更新 `.editorconfig`（`root = true`）。
2. 用 `[*.cs]`（及需要的 `*.{cs,vb}`）分段配置。
3. 优先配置高共识项：缩进、换行、Charset、using 顺序、文件头（若团队需要）。
4. 风格偏好用 `dotnet_style_*` / `csharp_style_*`；诊断用 `dotnet_diagnostic.*.severity`。
5. 命名规则也可在 EditorConfig 表达，但**语义命名争议**仍以 `naming-conventions` 技能决策为准。

### Step 3: 构建强制（可选）

在 `Directory.Build.props` 或项目中：

```xml
<PropertyGroup>
  <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
  <!-- 可选：分析器在构建时生效 -->
  <AnalysisLevel>latest</AnalysisLevel>
  <AnalysisMode>Default</AnalysisMode>
</PropertyGroup>
```

先在一个项目试点，再推广到全仓。

### Step 4: 应用格式

```bash
dotnet restore
dotnet format <solution-or-project> --severity warn
dotnet format <solution-or-project> --verify-no-changes
dotnet build
```

大仓按项目分批；审查 diff，排除生成代码目录。

### Step 5: CI

- PR 任务增加 `dotnet format --verify-no-changes`（或等效）。
- 文档说明：本地先 format 再推送。

## Examples

### 最小可用 `.editorconfig` 片段

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.cs]
indent_style = space
indent_size = 4

# Organization
dotnet_sort_system_directives_first = true
dotnet_separate_import_directive_groups = true

# var preferences (example — match your team)
csharp_style_var_for_built_in_types = true:suggestion
csharp_style_var_when_type_is_apparent = true:suggestion
csharp_style_var_elsewhere = false:suggestion

# Namespace
csharp_style_namespace_declarations = file_scoped:suggestion

# Avoid this. when unnecessary
dotnet_style_qualification_for_field = false:suggestion
dotnet_style_qualification_for_property = false:suggestion
dotnet_style_qualification_for_method = false:suggestion

# Example diagnostic severity
dotnet_diagnostic.IDE0005.severity = warning
dotnet_diagnostic.IDE0055.severity = warning
```

### 私有字段命名规则（EditorConfig，与 naming-conventions 对齐）

```ini
dotnet_naming_rule.private_fields_should_be_camel_case.severity = warning
dotnet_naming_rule.private_fields_should_be_camel_case.symbols = private_fields
dotnet_naming_rule.private_fields_should_be_camel_case.style = prefix_underscore

dotnet_naming_symbols.private_fields.applicable_kinds = field
dotnet_naming_symbols.private_fields.applicable_accessibilities = private

dotnet_naming_style.prefix_underscore.required_prefix = _
dotnet_naming_style.prefix_underscore.capitalization = camel_case
```

### Directory.Build.props 片段

```xml
<Project>
  <PropertyGroup>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
  </PropertyGroup>
</Project>
```

## Validation

- [ ] 存在合适的 `.editorconfig`（`root = true` 仅一处根声明）
- [ ] `dotnet format --verify-no-changes` 在约定范围内通过
- [ ] `dotnet build` 无新增不可接受的风格错误（若已 Enforce）
- [ ] 生成代码 / `obj` / `bin` 未被误格式化
- [ ] CI（若有）与本地命令一致
- [ ] 命名语义决策未与 `naming-conventions` 冲突

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| 多个 `root = true` 互相覆盖 | 只在真正根保留一个 root |
| 全仓突然 `severity = error` | 先 suggestion/warning，分批提升 |
| format 改动生成文件 | 排除 `**/*.g.cs`、工具输出目录 |
| 只改 EditorConfig 不跑 format | 配置 + `dotnet format` + CI verify |
| 把行为分析器（CA）与格式混为一谈 | 分清 style IDE* 与质量 CA*；分别定级 |
| CRLF/LF 混用导致无意义 diff | 统一 `end_of_line` 并配置 Git |

## References

- 选项速查：`references/editorconfig-essentials.md`
- 相关：`programming/csharp/naming-conventions`、`programming/csharp/modern-csharp`
- [Code style rule options](https://learn.microsoft.com/dotnet/fundamentals/code-analysis/code-style-rule-options)
- [EditorConfig naming conventions](https://learn.microsoft.com/dotnet/fundamentals/code-analysis/style-rules/naming-rules)
- [dotnet format](https://learn.microsoft.com/dotnet/core/tools/dotnet-format)
- [Enforce code style on build](https://learn.microsoft.com/dotnet/core/project-sdk/msbuild-props#enforcecodestyleinbuild)
