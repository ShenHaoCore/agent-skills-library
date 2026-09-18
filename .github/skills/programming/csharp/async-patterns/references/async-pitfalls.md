# Async Pitfalls Reference

Aligned with common .NET guidance (TAP, ConfigureAwait, cancellation).

## Sync-over-async

| Pattern | Risk | Prefer |
| :--- | :--- | :--- |
| `.Result` / `.Wait()` | 死锁、线程池饥饿 | 异步一路到底 |
| `Task.Run(...).Result` in request path | 隐藏阻塞、AggregateException | 直接 await |
| `GetAwaiter().GetResult()` | 与 Wait 类似 | await |

## Library vs application

| Rule | Library | Application |
| :--- | :--- | :--- |
| Wrap sync CPU in `Task.Run` | **Don't** expose as async API | Caller may `await Task.Run(...)` |
| Sync wrapper over async (`.Result`) | **Don't** | **Don't** |
| `ConfigureAwait(false)` | Yes | ASP.NET Core usually optional |

## async void

- 异常无法被调用方 await
- 测试困难
- 仅限 UI 事件：`async void Button_Click(...)`，事件内务必 try/catch

## ConfigureAwait

| Context | Guidance |
| :--- | :--- |
| 可复用类库 | `ConfigureAwait(false)` |
| ASP.NET Core 应用 | 通常直接 await |
| 旧版 ASP.NET / UI 需回原上下文 | 末级可省略 false；中间层仍建议 false |

## Cancellation

- 链接外部 token + 超时：`CreateLinkedTokenSource` + `CancelAfter`
- `using`/`Dispose` `CancellationTokenSource`
- 已取消后勿继续写共享状态
- 区分超时与用户取消仅在需要不同 UX 时包装

## ValueTask

- 热路径、常同步完成 → 考虑 `ValueTask`/`ValueTask<T>`
- 不要多次 await；需要缓存/多次消费 → `.AsTask()`
- 不要在一般业务代码默认全面改 ValueTask

## Concurrency vs Parallelism

- **Concurrency**：交错执行，多用于 IO（`WhenAll`）
- **Parallelism**：多核 CPU（`Parallel`、PLINQ、`Parallel.ForEachAsync`）
- 对异步 IO 使用同步 `Parallel.ForEach` 通常错误

## Bounded concurrency

```csharp
using var gate = new SemaphoreSlim(8);
var tasks = items.Select(async item =>
{
    await gate.WaitAsync(ct).ConfigureAwait(false);
    try { await ProcessAsync(item, ct).ConfigureAwait(false); }
    finally { gate.Release(); }
});
await Task.WhenAll(tasks).ConfigureAwait(false);
```

## Channels

优先 `System.Threading.Channels` 做生产者-消费者，而不是手写阻塞队列 + 手动线程。

## Detection helpers

```bash
rg -n --glob '*.cs' 'async void' -g '!**/bin/**' -g '!**/obj/**'
rg -n --glob '*.cs' '\.Result|\.Wait\(' -g '!**/bin/**' -g '!**/obj/**'
```

`.Result` 误报多（非 Task 属性也叫 Result）——需结合类型判断。
