# fuchsia-trailing-return

Functions that have trailing returns are disallowed, except for those  
using decltype specifiers and lambda with otherwise unutterable return  
types.

禁止使用尾置返回类型（trailing return）的函数，除非该函数使用了 decltype 说明符，或是 lambda 表达式且其返回类型无法直接表达。

For example:

例如：

```c++
// No warning
int add_one(const int arg) { return arg; }

// 无警告
int add_one(const int arg) { return arg; }

// Warning
auto get_add_one() -> int (*)(const int) {
  return add_one;
}

// 警告
auto get_add_one() -> int (*)(const int) {
  return add_one;
}
```

Exceptions are made for lambdas and decltype specifiers:

对于 lambda 表达式和 decltype 说明符是允许的例外：

```c++
// No warning
auto lambda = [](double x, double y) -> double {return x + y;};

// 无警告
auto lambda = [](double x, double y) -> double {return x + y;};

// No warning
template <typename T1, typename T2>
auto fn(const T1 &lhs, const T2 &rhs) -> decltype(lhs + rhs) {
  return lhs + rhs;
}

// 无警告
template <typename T1, typename T2>
auto fn(const T1 &lhs, const T2 &rhs) -> decltype(lhs + rhs) {
  return lhs + rhs;
}
```

See the features disallowed in Fuchsia at  
https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en

请参阅 Fuchsia 中不允许使用的特性：  
https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=zh-cn
