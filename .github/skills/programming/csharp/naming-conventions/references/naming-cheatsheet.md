# C# Naming Cheatsheet

Default when the repository has no stronger local convention.

## Casing

| Element | Case |
| :--- | :--- |
| Namespaces, types | PascalCase |
| Interfaces | `I` + PascalCase |
| Methods, properties, events, public fields | PascalCase |
| Parameters, locals | camelCase |
| Private instance fields | `_` + camelCase |
| Constants | PascalCase |
| Type parameters | `T` or `TName` |

## Async

- Awaitable-returning methods: `XxxAsync`
- Event handlers: usually no `Async` suffix
- Do not mark sync methods `Async`

## Booleans

Prefer `Is` / `Can` / `Has` / `Should` prefixes: `IsEnabled`, `CanSave`.

## Enums

- Type name: singular PascalCase (`OrderStatus`)
- Members: PascalCase, no redundant type prefix (`Pending` not `OrderStatusPending` unless required for clarity)

## Acronyms

- Two letters: often uppercase (`IO`, `ID` in older APIs — prefer `Id` in new .NET style where framework does)
- Longer: treat as word (`Html`, `Xml`, `Json`)

## Do not

- Hungarian notation (`strName`, `iCount`)
- Meaningless suffixes (`XxxManagerClass`)
- Rename published wire contracts without mapping attributes
