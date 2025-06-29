# readability-redundant-inline-specifier

Detects redundant inline specifiers on function and variable declarations.

检测函数和变量声明中冗余的 inline 说明符。

Examples:

示例：

```c++
constexpr inline void f() {}
```

In the example above the keyword inline is redundant since constexpr functions are implicitly inlined

在上面的示例中，inline 关键字是冗余的，因为 constexpr 函数会被隐式地内联。

```c++
class MyClass {
    inline void myMethod() {}
};
```

In the example above the keyword inline is redundant since member functions defined entirely inside a class/struct/union definition are implicitly inlined.

在上面的示例中，inline 关键字是冗余的，因为完全在类/结构体/联合体定义内部定义的成员函数会被隐式地内联。

## Options

## 选项

::: option
StrictMode

If set to true, the check will also flag functions and variables that already have internal linkage as redundant. Default is false.

如果设置为 true，该检查还会将已经具有内部链接的函数和变量标记为冗余。默认值为 false。
:::
