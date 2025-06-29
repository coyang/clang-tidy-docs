# llvmlibc-implementation-in-namespace

Checks that all declarations in the llvm-libc implementation are within  
the correct namespace.

检查 llvm-libc 实现中的所有声明是否位于正确的命名空间中。

```c++
// Implementation inside the LIBC_NAMESPACE_DECL namespace.
// Correct if:
// - LIBC_NAMESPACE_DECL is a macro
// - LIBC_NAMESPACE_DECL expansion starts with `[[gnu::visibility("hidden")]] __llvm_libc`
namespace LIBC_NAMESPACE_DECL {
    void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}
    // Namespaces within LIBC_NAMESPACE_DECL namespace are allowed.
    namespace inner {
        int localVar = 0;
    }
    // Functions with C linkage are allowed.
    extern "C" void str_fuzz() {}
}

// 在 LIBC_NAMESPACE_DECL 命名空间中的实现。
// 正确的情况：
// - LIBC_NAMESPACE_DECL 是一个宏
// - LIBC_NAMESPACE_DECL 展开后以 `[[gnu::visibility("hidden")]] __llvm_libc` 开头
namespace LIBC_NAMESPACE_DECL {
    void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}
    // 允许在 LIBC_NAMESPACE_DECL 命名空间中嵌套其他命名空间。
    namespace inner {
        int localVar = 0;
    }
    // 允许使用 C 链接方式的函数。
    extern "C" void str_fuzz() {}
}

// Incorrect: implementation not in the LIBC_NAMESPACE_DECL namespace.
void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}

// 错误：实现不在 LIBC_NAMESPACE_DECL 命名空间中。
void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}

// Incorrect: outer most namespace is not the LIBC_NAMESPACE_DECL macro.
namespace something_else {
    void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}
}

// 错误：最外层命名空间不是 LIBC_NAMESPACE_DECL 宏。
namespace something_else {
    void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}
}

// Incorrect: outer most namespace expansion does not start with `[[gnu::visibility("hidden")]] __llvm_libc`.
#define LIBC_NAMESPACE_DECL custom_namespace
namespace LIBC_NAMESPACE_DECL {
    void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}
}

// 错误：最外层命名空间展开后不是以 `[[gnu::visibility("hidden")]] __llvm_libc` 开头。
#define LIBC_NAMESPACE_DECL custom_namespace
namespace LIBC_NAMESPACE_DECL {
    void LLVM_LIBC_ENTRYPOINT(strcpy)(char *dest, const char *src) {}
}
```
