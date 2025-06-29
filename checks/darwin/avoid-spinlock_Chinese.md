# darwin-avoid-spinlock

Finds usages of OSSpinlock, which is deprecated due to potential livelock problems.

查找 OSSpinlock 的使用方式，该 API 已被弃用，因为可能导致活锁问题。

This check will detect following function invocations:

此检查将检测以下函数调用：

- OSSpinlockLock

- OSSpinlockTry

- OSSpinlockUnlock

The corresponding information about the problem of OSSpinlock:  
关于 OSSpinlock 问题的相关信息：

<https://blog.postmates.com/why-spinlocks-are-bad-on-ios-b69fc5221058>
