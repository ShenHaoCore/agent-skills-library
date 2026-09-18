---
name: modern-js
description: >
  Guides modern JavaScript idioms: ES modules, optional chaining, nullish
  coalescing, async/await, and contemporary standard-library usage.
  USE FOR: modernizing JS to ESM and async/await; optional chaining / ?? ;
  immutable shallow updates; unifying import/export style in Node or browsers.
  关键词：JavaScript、现代 JS、ES modules、optional chaining、nullish coalescing、
  async await、ES202x。
  DO NOT USE FOR: React/Vue/Svelte component architecture deep-dives;
  TypeScript-only compiler/tooling setups; Python or C# async skills.
license: MIT
---

# Modern JavaScript

用当代 JS 语法与模块化习惯编写清晰、安全的脚本与应用逻辑。

## When to Use

- 将回调/旧语法改为 async/await 与 ESM
- 使用可选链、空值合并等现代特性
- 统一模块导入导出与命名

## When Not to Use

- 框架组件架构（React/Vue 等）专项
- 纯 TypeScript 工程配置（tsconfig/路径别名）为主任务
- 其他语言的异步模型

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标文件或包 | Yes | 要现代化的 JS |
| 模块系统 | Recommended | ESM（`type: module`）或 CJS |
| 运行时 | Recommended | Node LTS 或目标浏览器基线 |

## Workflow

1. 确认 `package.json` 的 `type` 与现有 import 风格。
2. 新代码优先 ESM；与 CJS 互操时明确边界。
3. 异步用 `async/await` + 明确错误处理；勿忽略 Promise。
4. 用 `?.` 与 `??`；慎用 `||` 对 `0`/`''` 的误伤。
5. 不可变更新：展开/浅拷贝，避免共享可变状态。
6. 避免无必要的 `var` 与 callback hell（除非维护遗留）。

## Examples

```javascript
export async function loadUser(api, id, { signal } = {}) {
  const res = await api.get(`/users/${id}`, { signal });
  const name = res?.profile?.displayName ?? "Unknown";
  return { id, name, tags: [...(res.tags ?? [])] };
}
```

## Validation

- [ ] 目标运行时可解析所用语法
- [ ] 无未处理的 Promise / 浮动 async
- [ ] ESM/CJS 边界清晰，无循环依赖恶化
- [ ] （可选）现有测试或手动冒烟通过

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| `||` 把合法假值当缺省 | 用 `??` |
| 混用 require/import 无边界 | 统一或做显式互操层 |
| 忽略 fetch/API 错误 | 检查 status / try-catch |
| 原地 mutate 共享对象 | 返回新对象/数组 |

## References

- 提示词辅助：`ai/prompt-engineering`
