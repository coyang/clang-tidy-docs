# cppcoreguidelines-avoid-reference-coroutine-parameters

Warns when a coroutine accepts reference parameters. After a coroutine suspend point, references could be dangling and no longer valid. Instead, pass parameters as values.

当协程接受引用参数时会发出警告。在协程的挂起点之后，引用可能会变成悬空引用而不再有效。应改为按值传递参数。

Examples:

示例：

```c++
std::future<int> someCoroutine(int& val) {
  co_await ...;
  // When the coroutine is resumed, 'val' might no longer be valid.
  if (val) ...
}
```

```c++
std::future<int> someCoroutine(int& val) {
  co_await ...;
  // 当协程恢复执行时，'val' 可能已经无效。
  if (val) ...
}
```

This check implements CP.53 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 CP.53 条款。

[CP.53](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rcoro-reference-parameters)
