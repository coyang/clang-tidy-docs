# misc-coroutine-hostile-raii

Detects when objects of certain hostile RAII types persists across suspension points in a coroutine. Such hostile types include scoped-lockable types and types belonging to a configurable denylist.

检测某些具有敌意的 RAII 类型的对象在协程中跨越挂起点（suspension point）持续存在的情况。这些具有敌意的类型包括作用域锁类型（scoped-lockable types）以及属于可配置拒绝列表（denylist）中的类型。

Some objects require that they be destroyed on the same thread that created them. Traditionally this requirement was often phrased as "must be a local variable", under the assumption that local variables always work this way. However this is incorrect with C++20 coroutines, since an intervening co_await may cause the coroutine to suspend and later be resumed on another thread.

某些对象要求必须在创建它们的同一线程上销毁。传统上，这种要求通常被表述为“必须是局部变量”，基于局部变量总是在同一线程上创建和销毁的假设。然而，在 C++20 协程中，这种假设是不正确的，因为中间的 co_await 可能导致协程挂起，并在另一个线程上恢复执行。

The lifetime of an object that requires being destroyed on the same thread must not encompass a co_await or co_yield point. If you create/destroy an object, you must do so without allowing the coroutine to suspend in the meantime.

要求在同一线程上销毁的对象，其生命周期不能跨越 co_await 或 co_yield 点。如果你创建/销毁一个对象，必须确保在此期间协程不会挂起。

Following types are considered as hostile:

以下类型被视为具有敌意的类型：

- Scoped-lockable types: A scoped-lockable object persisting across a suspension point is problematic as the lock held by this object could be unlocked by a different thread. This would be undefined behaviour. This includes all types annotated with the scoped_lockable attribute.
- Types belonging to a configurable denylist.

- 作用域锁类型（Scoped-lockable types）：如果一个作用域锁对象在协程挂起点之后仍然存在，那么它持有的锁可能会被另一个线程解锁，这将导致未定义行为。这包括所有带有 scoped_lockable 属性的类型。
- 属于可配置拒绝列表中的类型。

```c++
// Call some async API while holding a lock.
task coro() {
  const std::lock_guard l(&mu_);

  // Oops! The async Bar function may finish on a different
  // thread from the one that created the lock_guard (and called
  // Mutex::Lock). After suspension, Mutex::Unlock will be called on the wrong thread.
  co_await Bar();
}
```

```c++
// 在持有锁的情况下调用某个异步 API。
task coro() {
  const std::lock_guard l(&mu_);

  // 糟糕！异步函数 Bar 可能会在一个不同的线程上完成，
  // 与创建 lock_guard（并调用 Mutex::Lock）时的线程不同。
  // 挂起之后，Mutex::Unlock 将在错误的线程上被调用。
  co_await Bar();
}
```

## Options

## 选项

::: option
RAIITypesList

A semicolon-separated list of qualified types which should not be allowed to persist across suspension points. Eg: my::lockable;a::b;::my::other::lockable The default value of this option is std::lock_guard;std::scoped_lock.

一个由分号分隔的限定类型列表，这些类型不允许在协程挂起点之间持续存在。例如：my::lockable;a::b;::my::other::lockable。该选项的默认值为 std::lock_guard;std::scoped_lock。
:::

::: option
AllowedAwaitablesList

A semicolon-separated list of qualified types of awaitables types which can be safely awaited while having hostile RAII objects in scope.

一个由分号分隔的可等待类型（awaitable types）的限定类型列表，在这些类型的 co_await 表达式中，即使存在具有敌意的 RAII 对象也可以安全地等待。

co_await-ing an expression of awaitable type is considered safe if the awaitable type is part of this list. RAII objects persisting across such a co_await expression are considered safe and hence are not flagged.

如果 co_await 的表达式的类型属于该列表中的 awaitable 类型，则认为该 co_await 是安全的。跨越这种 co_await 表达式的 RAII 对象也被视为安全，因此不会被标记。

Example usage:

示例用法：

```c++
// Consider option AllowedAwaitablesList = "safe_awaitable"
struct safe_awaitable {
  bool await_ready() noexcept { return false; }
  void await_suspend(std::coroutine_handle<>) noexcept {}
  void await_resume() noexcept {}
};
auto wait() { return safe_awaitable{}; }

task coro() {
  // This persists across both the co_await's but is not flagged
  // because the awaitable is considered safe to await on.
  const std::lock_guard l(&mu_);
  co_await safe_awaitable{};
  co_await wait();
}
```

```c++
// 假设选项 AllowedAwaitablesList = "safe_awaitable"
struct safe_awaitable {
  bool await_ready() noexcept { return false; }
  void await_suspend(std::coroutine_handle<>) noexcept {}
  void await_resume() noexcept {}
};
auto wait() { return safe_awaitable{}; }

task coro() {
  // 该对象在两个 co_await 之间持续存在，但不会被标记，
  // 因为这些 awaitable 被认为是安全的。
  const std::lock_guard l(&mu_);
  co_await safe_awaitable{};
  co_await wait();
}
```

Eg: my::safe::awaitable;other::awaitable Default is an empty string.

例如：my::safe::awaitable;other::awaitable。默认值为空字符串。
:::
