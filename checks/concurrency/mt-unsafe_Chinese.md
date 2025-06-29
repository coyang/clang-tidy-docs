# concurrency-mt-unsafe

Checks for some thread-unsafe functions against a black list of known-to-be-unsafe functions. Usually they access static variables without synchronization (e.g. gmtime(3)) or utilize signals in a racy way. The set of functions to check is specified with the FunctionSet option.

检查一些已知线程不安全函数的黑名单。通常这些函数在没有同步的情况下访问静态变量（例如 gmtime(3)），或者以竞争的方式使用信号。要检查的函数集合通过 FunctionSet 选项指定。

Note that using some thread-unsafe functions may be still valid in concurrent programming if only a single thread is used (e.g. setenv(3)), however, some functions may track a state in global variables which would be clobbered by subsequent (non-parallel, but concurrent) calls to a related function. E.g. the following code suffers from unprotected accesses to a global state:

请注意，如果只使用单个线程，在并发编程中使用某些线程不安全的函数仍然可能是合法的（例如 setenv(3)）。然而，一些函数可能会在全局变量中维护状态，而该状态可能会被后续（非并行但并发）的相关函数调用破坏。例如，以下代码存在对全局状态的非保护访问问题：

```c++
// getnetent(3) maintains global state with DB connection, etc.
// If a concurrent green thread calls getnetent(3), the global state is corrupted.
netent = getnetent();
yield();
netent = getnetent();
```

```c++
// getnetent(3) 使用数据库连接等维护全局状态。
// 如果一个并发的绿色线程调用 getnetent(3)，全局状态将被破坏。
netent = getnetent();
yield();
netent = getnetent();
```

Examples:

示例：

```c++
tm = gmtime(timep); // uses a global buffer

sleep(1); // implementation may use SIGALRM
```

```c++
tm = gmtime(timep); // 使用全局缓冲区

sleep(1); // 实现可能使用 SIGALRM
```

## Options

## 选项

::: option
FunctionSet

Specifies which functions in libc should be considered thread-safe, possible values are posix, glibc, or any.

指定在 libc 中哪些函数应被视为线程安全的，可能的取值包括 posix、glibc 或 any。

posix means POSIX defined thread-unsafe functions. POSIX.1-2001 in "2.9.1 Thread-Safety" defines that all functions specified in the standard are thread-safe except a predefined list of thread-unsafe functions.

posix 表示 POSIX 定义的线程不安全函数。POSIX.1-2001 在“2.9.1 线程安全性”中定义，标准中指定的所有函数都是线程安全的，除了一个预定义的线程不安全函数列表。

Glibc defines some of them as thread-safe (e.g. dirname(3)), but adds non-POSIX thread-unsafe ones (e.g. getopt_long(3)). Glibc's list is compiled from GNU web documentation with a search for MT-Safe tag: https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html

Glibc 将其中一些函数定义为线程安全（例如 dirname(3)），但也添加了一些非 POSIX 的线程不安全函数（例如 getopt_long(3)）。Glibc 的列表是通过在 GNU 网站文档中搜索 MT-Safe 标签整理而成的： https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html

If you want to identify thread-unsafe API for at least one libc or unsure which libc will be used, use any (default).

如果您希望识别至少一个 libc 中的线程不安全 API，或不确定将使用哪个 libc，请使用 any（默认值）。
:::
