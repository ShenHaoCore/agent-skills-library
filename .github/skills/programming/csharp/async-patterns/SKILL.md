---
name: async-patterns
description: >
  Guides reliable C# async/await (TAP): Task/ValueTask, CancellationToken,
  ConfigureAwait, concurrency vs parallelism, and sync-over-async fixes.
  USE FOR: writing or reviewing async C# methods; deadlocks from .Result/.Wait();
  passing CancellationToken; library ConfigureAwait(false); Task vs ValueTask;
  Task.WhenAll/WhenAny; IAsyncEnumerable; Channels; async void event handlers.
  关键词：异步、async、await、Task、ConfigureAwait、ValueTask、CancellationToken、
  async void、死锁、并行与并发、IAsyncEnumerable。
  DO NOT USE FOR: general C# syntax or records/NRT (use modern-csharp);
  EF Core migrations (use ef-core-migrations); non-C# async (JS/Python);
  CPU-only Parallel.For without async I/O concerns.
license: MIT
metadata:
  inspired-by: https://github.com/dotnet/skills
  microsoft-learn: https://learn.microsoft.com/dotnet/csharp/asynchronous-programming/
---

# C# Async Patterns

写出可组合、可取消、不阻塞线程的 C# 异步代码（Task-based Asynchronous Pattern）。需要现代语言特性时交叉加载 `modern-csharp`。

## When to Use

- 编写或审查 `async`/`await` 方法
- 排查 UI / 旧 ASP.NET 死锁或 sync-over-async
- 为公共 API 传递 `CancellationToken`、超时或协作式取消
- 选择 `Task` vs `ValueTask`、类库是否 `ConfigureAwait(false)`
- 使用 `Task.WhenAll` / `WhenAny`、`IAsyncEnumerable`、`Channel`
- 区分 IO 并发与 CPU 并行

## When Not to Use

- 只要 modern C# 语法（record、pattern matching、NRT）→ `modern-csharp`
- EF Core 迁移 / `dotnet ef` → `ef-core-migrations`
- 非 C# 异步（JavaScript、Python）
- 纯同步 CPU 算法且无异步边界（直接给同步 API，由调用方决定是否 `Task.Run`）

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| 目标代码或文件路径 | Recommended | 要编写/审查的异步方法 |
| 宿主类型 | Recommended | 类库 / ASP.NET Core / UI（WinForms、WPF、旧 ASP.NET） |
| TFM | Optional | 如 `net8.0`；影响推荐 API |

## Workflow

### Step 1: 分类场景（IO-bound vs CPU-bound）

| 问题 | 场景 | 做法 |
| --- | --- | --- |
| 等待外部结果（HTTP、DB、文件）？ | **IO-bound** | `async`/`await` 直接调异步 API；**不要**用 `Task.Run` 包一层 |
| 昂贵计算？ | **CPU-bound** | 同步实现核心逻辑；需要时由**应用层** `await Task.Run(...)`，类库不要暴露假异步包装 |

### Step 2: 命名与返回类型（TAP）

1. 异步方法名加 `Async` 后缀；与同步对应物对齐（`GetData` → `GetDataAsync`）。
2. 有返回值 → `Task<T>` / `ValueTask<T>`；无返回值 → `Task` / `ValueTask`。
3. **禁止** `async void`（仅 UI 事件处理器例外）。
4. 仅转发已有 `Task` 时可省略多余 `async`/`await`（elision），注意异常堆栈与上下文差异。

### Step 3: 取消与超时

1. 公共异步方法接受 `CancellationToken cancellationToken = default`。
2. Token 一路传到 IO API；循环中 `ThrowIfCancellationRequested()`。
3. 超时：`CancelAfter` 或链接 token；`using` 释放 `CancellationTokenSource`。
4. 让 `OperationCanceledException` 向上传播；只在编排边界处理。

### Step 4: ConfigureAwait 与宿主

| 宿主 | 建议 |
| --- | --- |
| 可复用类库 | `await x.ConfigureAwait(false)` |
| ASP.NET Core 应用代码 | 通常直接 `await` |
| UI / 旧 ASP.NET（有 SynchronizationContext） | 避免 `.Result`/`.Wait()`；中间层库仍用 `false` |

### Step 5: ValueTask / 并发 / 流

- **ValueTask**：热路径、经常同步完成、需减分配时再用；不要多次 await 同一实例。
- **IO 并发**：`Task.WhenAll` / `WhenAny`；限制并发用 `SemaphoreSlim`。
- **生产者-消费者**：优先 `System.Threading.Channels` + `await foreach`。
- **异步流**：`IAsyncEnumerable<T>` + `await foreach`。

### Step 6: 验证

按 Validation 清单检查；有解决方案时运行相关测试。

## Examples

### 正确：可取消 HTTP（类库）

```csharp
public async Task<string> DownloadAsync(
    HttpClient http,
    string url,
    CancellationToken cancellationToken = default)
{
    using var response = await http
        .GetAsync(url, HttpCompletionOption.ResponseHeadersRead, cancellationToken)
        .ConfigureAwait(false);

    response.EnsureSuccessStatusCode();
    return await response.Content
        .ReadAsStringAsync(cancellationToken)
        .ConfigureAwait(false);
}
```

### 错误：库内假异步 + sync-over-async

```csharp
// BAD — 类库不要用 Task.Run 包装纯同步 CPU
public Task<int> ComputeHashAsync(byte[] data) =>
    Task.Run(() => ComputeHash(data));

// BAD — 阻塞
var data = DownloadAsync(http, url).Result;
```

### 并发 IO

```csharp
var tasks = urls.Select(u => DownloadAsync(http, u, ct));
string[] pages = await Task.WhenAll(tasks).ConfigureAwait(false);
```

### 超时

```csharp
using var cts = CancellationTokenSource.CreateLinkedTokenSource(externalCt);
cts.CancelAfter(TimeSpan.FromSeconds(30));
await DownloadAsync(http, url, cts.Token).ConfigureAwait(false);
```

### ValueTask 缓存命中

```csharp
public ValueTask<User> GetUserAsync(string id, CancellationToken ct = default)
{
    if (_cache.TryGetValue(id, out var user))
        return ValueTask.FromResult(user);

    return new ValueTask<User>(LoadFromDbAsync(id, ct));
}
```

### Channel 消费者

```csharp
await foreach (var item in channel.Reader.ReadAllAsync(ct))
{
    await ProcessAsync(item, ct).ConfigureAwait(false);
}
```

## Validation

- [ ] 无 `async void`（事件除外）
- [ ] 异步路径无 `.Result` / `.Wait()` / `.GetAwaiter().GetResult()`
- [ ] 公共异步 API 接受并向下传递 `CancellationToken`
- [ ] 类库 await 使用 `ConfigureAwait(false)`（适用时）
- [ ] 未在类库用 `Task.Run` 包装同步方法冒充异步
- [ ] 明确是 IO 并发还是 CPU 并行
- [ ] （可选）`dotnet build` 通过；相关测试绿色

```bash
# 粗查 async void（需人工排除事件）
rg -n --glob '*.cs' 'async void' -g '!**/bin/**' -g '!**/obj/**'
```

## Common Pitfalls

| Pitfall | Fix |
| --- | --- |
| sync-over-async 死锁 | 异步一路到底；UI/旧 ASP.NET 尤其危险 |
| `async void` 异常无法观察 | 改为 `async Task`；事件内包 try/catch |
| 类库 `Task.Run` 包装同步 | 暴露同步 API，由调用方决定卸载 |
| 忽略 CancellationToken | 参数 + 传到 IO + 循环检查 |
| 多次 await 同一 `ValueTask` | 一次消费，或 `.AsTask()` |
| `WhenAll` 无界扇出 | `SemaphoreSlim` 限制并发度 |

## References

- 详细陷阱：`references/async-pitfalls.md`
- 相关技能：`programming/csharp/modern-csharp`、`programming/csharp/naming-conventions`
- [Asynchronous programming scenarios](https://learn.microsoft.com/dotnet/csharp/asynchronous-programming/async-scenarios)
- [The Task asynchronous programming model](https://learn.microsoft.com/dotnet/csharp/asynchronous-programming/task-asynchronous-programming-model)
- [ConfigureAwait FAQ](https://devblogs.microsoft.com/dotnet/configureawait-faq/)
