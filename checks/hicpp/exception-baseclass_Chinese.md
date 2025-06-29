# hicpp-exception-baseclass

Ensure that every value that in a throw expression is an instance of std::exception.

确保 throw 表达式中的每个值都是 std::exception 的实例。

This enforces rule 15.1 of the High Integrity C++ Coding Standard.

这符合高完整性 C++ 编码标准中的规则 15.1。

```c++
class custom_exception {};

void throwing() noexcept(false) {
  // Problematic throw expressions.
  // 有问题的 throw 表达式。
  throw int(42);
  throw custom_exception();
}

class mathematical_error : public std::exception {};

void throwing2() noexcept(false) {
  // These kind of throws are ok.
  // 这些类型的 throw 表达式是可以接受的。
  throw mathematical_error();
  throw std::runtime_error();
  throw std::exception();
}
```
