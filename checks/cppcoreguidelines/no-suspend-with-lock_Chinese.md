# cppcoreguidelines-no-suspend-with-lock

Flags coroutines that suspend while a lock guard is in scope at the suspension point.

标记在挂起点仍处于锁守卫作用域内的协程。

When a coroutine suspends, any mutexes held by the coroutine will remain locked until the coroutine resumes and eventually destructs the lock guard. This can lead to long periods with a mutex held and runs the risk of deadlock.

当协程挂起时，其持有的任何互斥锁将保持锁定状态，直到协程恢复并最终销毁锁守卫。这可能导致互斥锁长时间被占用，并带来死锁的风险。

Instead, locks should be released before suspending a coroutine.

因此，应在协程挂起之前释放锁。

This check only checks suspending coroutines while a lock_guard is in scope; it does not consider manual locking or unlocking of mutexes, e.g., through calls to std::mutex::lock().

此检查仅检测在 lock_guard 作用域内挂起的协程；它不考虑通过 std::mutex::lock() 等方式手动加锁或解锁的情况。

Examples:

示例：

```c++
future bad_coro() {
  std::lock_guard lock{mtx};
  ++some_counter;
  co_await something(); // Suspending while holding a mutex
}

future good_coro() {
  {
    std::lock_guard lock{mtx};
    ++some_counter;
  }
  // Destroy the lock_guard to release the mutex before suspending the coroutine
  co_await something(); // Suspending while holding a mutex
}
```

```c++
future bad_coro() {
  std::lock_guard lock{mtx};
  ++some_counter;
  co_await something(); // 在持有互斥锁时挂起
}

future good_coro() {
  {
    std::lock_guard lock{mtx};
    ++some_counter;
  }
  // 在挂起协程之前销毁 lock_guard 以释放互斥锁
  co_await something(); // 在持有互斥锁时挂起
}
```

This check implements CP.52 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 CP.52 条款。

[CP.52](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rcoro-locks)
