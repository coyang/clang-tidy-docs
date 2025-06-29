# llvmlibc-callee-namespace

Checks all calls resolve to functions within correct namespace.

检查所有调用是否解析为正确命名空间中的函数。

```c++
// Implementation inside the LIBC_NAMESPACE namespace.
// Correct if:
// - LIBC_NAMESPACE is a macro
// - LIBC_NAMESPACE expansion starts with `__llvm_libc`
namespace LIBC_NAMESPACE {

// Allow calls with the fully qualified name.
LIBC_NAMESPACE::strlen("hello");

// Allow calls to compiler provided functions.
(void)__builtin_abs(-1);

// Bare calls are allowed as long as they resolve to the correct namespace.
strlen("world");

// Disallow calling into functions in the global namespace.
::strlen("!");

} // namespace LIBC_NAMESPACE
```

```c++
// 在 LIBC_NAMESPACE 命名空间中的实现。
// 正确的情况：
// - LIBC_NAMESPACE 是一个宏
// - LIBC_NAMESPACE 展开后以 `__llvm_libc` 开头
namespace LIBC_NAMESPACE {

// 允许使用完全限定名称的调用。
LIBC_NAMESPACE::strlen("hello");

// 允许调用编译器提供的函数。
(void)__builtin_abs(-1);

// 只要能解析到正确的命名空间，就允许裸调用。
strlen("world");

// 不允许调用全局命名空间中的函数。
::strlen("!");

} // namespace LIBC_NAMESPACE
```
