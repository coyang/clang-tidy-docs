# darwin-dispatch-once-nonstatic

Finds declarations of dispatch_once_t variables without static or global storage. The behavior of using dispatch_once_t predicates with automatic or dynamic storage is undefined by libdispatch, and should be avoided.

查找未使用 static 或全局存储的 dispatch_once_t 变量声明。将 dispatch_once_t 谓词与自动或动态存储一起使用的行为在 libdispatch 中是未定义的，应当避免。

It is a common pattern to have functions initialize internal static or global data once when the function runs, but programmers have been known to miss the static on the dispatch_once_t predicate, leading to an uninitialized flag value at the mercy of the stack.

一种常见的模式是在函数运行时初始化内部 static 或全局数据一次，但程序员有时会忘记在 dispatch_once_t 谓词上添加 static，导致标志值未初始化，依赖于栈的状态。

Programmers have also been known to make dispatch_once_t variables be members of structs or classes, with the intent to lazily perform some expensive struct or class member initialization only once; however, this violates the libdispatch requirements.

程序员有时也会将 dispatch_once_t 变量作为结构体或类的成员，意图仅在首次访问时延迟执行一些开销较大的结构体或类成员初始化；然而，这违反了 libdispatch 的要求。

See the discussion section of Apple's dispatch_once documentation for more information.

有关更多信息，请参阅 Apple 的 dispatch_once 文档中的讨论部分：
https://developer.apple.com/documentation/dispatch/1447169-dispatch_once
