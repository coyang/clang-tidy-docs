# fuchsia-default-arguments-calls

Warns if a function or method is called with default arguments.

如果函数或方法在调用时使用了默认参数，则会发出警告。

For example, given the declaration:

例如，给定如下声明：

```c++
int foo(int value = 5) { return value; }
```

A function call expression that uses a default argument will be diagnosed. Calling it without defaults will not cause a warning:

使用默认参数的函数调用表达式将会被诊断。若调用时未使用默认参数，则不会触发警告：

```c++
foo();  // warning
foo(0); // no warning
```

See the features disallowed in Fuchsia at  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en>

请参阅 Fuchsia 中不允许使用的特性：  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=zh-cn>
