# fuchsia-overloaded-operator

Warns if an operator is overloaded, except for the assignment (copy and move) operators.

如果重载了某个运算符（除了赋值运算符，包括拷贝赋值和移动赋值），则会发出警告。

For example:

例如：

```c++
int operator+(int);     // Warning
int operator+(int);     // 警告

B &operator=(const B &Other);  // No warning
B &operator=(const B &Other);  // 无警告

B &operator=(B &&Other) // No warning
B &operator=(B &&Other) // 无警告
```

See the features disallowed in Fuchsia at  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en>

请参阅 Fuchsia 中不允许使用的特性：  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=zh-cn>
