# modernize-unary-static-assert

The check diagnoses any static_assert declaration with an empty string literal and provides a fix-it to replace the declaration with a single-argument static_assert declaration.

该检查会诊断任何带有空字符串字面量的 static_assert 声明，并提供修复建议，将其替换为只带一个参数的 static_assert 声明。

The check is only applicable for C++17 and later code.

该检查仅适用于 C++17 及更高版本的代码。

The following code:

以下代码：

```c++
void f_textless(int a) {
  static_assert(sizeof(a) <= 10, "");
}
```

is replaced by:

将被替换为：

```c++
void f_textless(int a) {
  static_assert(sizeof(a) <= 10);
}
```
