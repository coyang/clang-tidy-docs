# readability-avoid-const-params-in-decls

Checks whether a function declaration has parameters that are top level const.

检查函数声明中是否存在顶层 const 的参数。

const values in declarations do not affect the signature of a function, so they should not be put there.

在函数声明中使用 const 并不会影响函数签名，因此不应在此处使用。

Examples:

示例：

```c++
void f(const string);   // Bad: const is top level.
                        // 错误：const 是顶层修饰符。

void f(const string&);  // Good: const is not top level.
                        // 正确：const 不是顶层修饰符。
```

## Options

## 选项

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，则该检查不会在宏内部发出警告。默认值为 true。
:::
