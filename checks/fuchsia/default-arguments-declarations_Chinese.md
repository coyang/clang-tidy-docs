# fuchsia-default-arguments-declarations

Warns if a function or method is declared with default parameters.

如果函数或方法在声明时使用了默认参数，将会触发警告。

For example, the declaration:

例如，以下声明：

```c++
int foo(int value = 5) { return value; }
```

will cause a warning.

将会触发警告。

See the features disallowed in Fuchsia at  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en>

请参阅 Fuchsia 中不允许使用的特性：  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=zh-cn>
