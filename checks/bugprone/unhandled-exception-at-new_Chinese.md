# bugprone-unhandled-exception-at-new

Finds calls to new with missing exception handler for std::bad_alloc.

查找在调用 new 时缺少对 std::bad_alloc 异常处理的情况。

Calls to new may throw exceptions of type std::bad_alloc that should be handled. Alternatively, the nonthrowing form of new can be used. The check verifies that the exception is handled in the function that calls new.

调用 new 可能会抛出 std::bad_alloc 类型的异常，因此应当进行处理。或者，也可以使用不会抛出异常的 new 形式。此检查会验证调用 new 的函数中是否对异常进行了处理。

If a nonthrowing version is used or the exception is allowed to propagate out of the function no warning is generated.

如果使用了不会抛出异常的版本，或者允许异常从函数中传播出去，则不会生成警告。

The exception handler is checked if it catches a std::bad_alloc or std::exception exception type, or all exceptions (catch-all). The check assumes that any user-defined operator new is either noexcept or may throw an exception of type std::bad_alloc (or one derived from it). Other exception class types are not taken into account.

检查会验证异常处理程序是否捕获了 std::bad_alloc 或 std::exception 类型的异常，或是使用了通用捕获（catch-all）。该检查假设任何用户自定义的 operator new 要么是 noexcept 的，要么可能抛出 std::bad_alloc 类型（或其派生类型）的异常。其他异常类型不会被考虑在内。

```c++
int *f() noexcept {
  int *p = new int[1000]; // warning: missing exception handler for allocation failure at 'new'
                          // 警告：在 'new' 分配失败时缺少异常处理
  // ...
  return p;
}
```

```c++
int *f1() { // not 'noexcept'
  int *p = new int[1000]; // no warning: exception can be handled outside
                          // of this function
                          // 无警告：异常可以在此函数外部处理
  // ...
  return p;
}

int *f2() noexcept {
  try {
    int *p = new int[1000]; // no warning: exception is handled
                            // 无警告：异常已被处理
    // ...
    return p;
  } catch (std::bad_alloc &) {
    // ...
  }
  // ...
}

int *f3() noexcept {
  int *p = new (std::nothrow) int[1000]; // no warning: "nothrow" is used
                                         // 无警告：使用了 "nothrow"
  // ...
  return p;
}
```
