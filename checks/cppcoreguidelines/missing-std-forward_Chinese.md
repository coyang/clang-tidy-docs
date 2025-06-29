# cppcoreguidelines-missing-std-forward

Warns when a forwarding reference parameter is not forwarded inside the function body.

当转发引用参数在函数体内未被正确转发时发出警告。

Example:

示例：

```c++
template <class T>
void wrapper(T&& t) {
  impl(std::forward<T>(t), 1, 2); // Correct
                                // 正确
}

template <class T>
void wrapper2(T&& t) {
  impl(t, 1, 2); // Oops - should use std::forward<T>(t)
                // 错误 - 应该使用 std::forward<T>(t)
}

template <class T>
void wrapper3(T&& t) {
  impl(std::move(t), 1, 2); // Also buggy - should use std::forward<T>(t)
                           // 同样错误 - 应该使用 std::forward<T>(t)
}

template <class F>
void wrapper_function(F&& f) {
  std::forward<F>(f)(1, 2); // Correct
                          // 正确
}

template <class F>
void wrapper_function2(F&& f) {
  f(1, 2); // Incorrect - may not invoke the desired qualified function operator
          // 错误 - 可能不会调用期望的限定函数调用运算符
}
```

This check implements F.19 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 F.19 条款。  
参考链接：[F.19](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rf-forward)
