# portability-std-allocator-const

Report use of std::vector<const T> (and similar containers of const elements). These are not allowed in standard C++, and should usually be std::vector<T> instead.

报告使用 std::vector<const T>（以及类似的包含 const 元素的容器）。这些在标准 C++ 中是不被允许的，通常应使用 std::vector<T> 代替。

Per C++ [allocator.requirements.general]: "T is any cv-unqualified object type", std::allocator<const T> is undefined. Many standard containers use std::allocator by default and therefore their const T instantiations are undefined.

根据 C++ 标准 [allocator.requirements.general]： “T 是任何未加 cv 限定的对象类型”，因此 std::allocator<const T> 是未定义的。许多标准容器默认使用 std::allocator，因此它们对 const T 的实例化是未定义的。

libc++ defines std::allocator<const T> as an extension which will be removed in the future.

libc++ 将 std::allocator<const T> 定义为一个扩展功能，但该功能将在未来被移除。

libstdc++ and MSVC do not support std::allocator<const T>:

libstdc++ 和 MSVC 不支持 std::allocator<const T>：

```c++
// libstdc++ has a better diagnostic since https://gcc.gnu.org/bugzilla/show_bug.cgi?id=48101
// libstdc++ 自 https://gcc.gnu.org/bugzilla/show_bug.cgi?id=48101 起提供了更好的诊断信息
std::deque<const int> deque; // error: static assertion failed: std::deque must have a non-const, non-volatile value_type
                             // 错误：静态断言失败：std::deque 必须具有非 const、非 volatile 的 value_type
std::set<const int> set;     // error: static assertion failed: std::set must have a non-const, non-volatile value_type
                             // 错误：静态断言失败：std::set 必须具有非 const、非 volatile 的 value_type
std::vector<int* const> vector; // error: static assertion failed: std::vector must have a non-const, non-volatile value_type
                                // 错误：静态断言失败：std::vector 必须具有非 const、非 volatile 的 value_type

// MSVC
// error C2338: static_assert failed: 'The C++ Standard forbids containers of const elements because allocator<const T> is ill-formed.'
// 错误 C2338：静态断言失败：‘C++ 标准禁止包含 const 元素的容器，因为 allocator<const T> 是格式错误的。’
```

Code bases only compiled with libc++ may accrue such undefined usage. This check finds such code and prevents backsliding while clean-up is ongoing.

仅使用 libc++ 编译的代码库可能会积累此类未定义的用法。此检查可发现此类代码，并在清理过程中防止回退。
