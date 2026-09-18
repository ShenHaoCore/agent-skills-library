# EditorConfig Essentials

## File placement

```text
repo-root/
  .editorconfig          # root = true
  Directory.Build.props  # optional EnforceCodeStyleInBuild
  src/...
```

## Categories

| Category | Keys (examples) |
| :--- | :--- |
| File hygiene | `charset`, `end_of_line`, `insert_final_newline`, `trim_trailing_whitespace` |
| Indent | `indent_style`, `indent_size` |
| Language style | `csharp_style_*`, `dotnet_style_*` |
| Naming | `dotnet_naming_rule.*`, `dotnet_naming_symbols.*`, `dotnet_naming_style.*` |
| Severities | `dotnet_diagnostic.<ID>.severity = none\|silent\|suggestion\|warning\|error` |

## Useful diagnostic IDs

| ID | Topic |
| :--- | :--- |
| IDE0005 | Remove unnecessary usings |
| IDE0055 | Formatting |
| IDE0161 | File-scoped namespace |
| IDE0040 | Accessibility modifiers |
| CA* | Quality/design analyzers (separate from pure format) |

## Commands

```bash
dotnet format                          # format solution in cwd
dotnet format style                    # style subset
dotnet format analyzers                # analyzer fixes when supported
dotnet format --verify-no-changes      # CI gate
```

## Rollout tip

1. Add EditorConfig as `suggestion`
2. `dotnet format` on one project
3. Raise hot rules to `warning`
4. Enable `EnforceCodeStyleInBuild` after CI is green
5. Only then elevate selected rules to `error`
