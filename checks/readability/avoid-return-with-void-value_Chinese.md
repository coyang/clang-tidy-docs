# readability-avoid-return-with-void-value

Finds return statements with void values used within functions with void result types.

查找在返回类型为 void 的函数中使用 void 值的 return 语句。

A function with a void return type is intended to perform a task without producing a return value. Return statements with expressions could lead to confusion and may miscommunicate the function's intended behavior.

返回类型为 void 的函数旨在执行某项任务而不返回任何值。带有表达式的 return 语句可能会引起混淆，并可能误导函数的预期行为。

Example:

示例：

```
void g();
void f() {
    // ...
    return g();
}
```

In a long function body, the return statement suggests that the function returns a value. However, return g(); is a combination of two statements that should be written as

在一个较长的函数体中，return 语句暗示函数会返回一个值。然而，return g(); 实际上是两个语句的组合，应该写成：

```
g();
return;
```

to make clear that g() is called and immediately afterwards the function returns (nothing).

这样可以更清楚地表明调用了 g()，然后函数立即返回（不返回任何值）。

In C, the same issue is detected by the compiler if the -Wpedantic mode is enabled.

在 C 语言中，如果启用了 -Wpedantic 模式，编译器也会检测到同样的问题。

## Options

## 选项

::: option
IgnoreMacros

The value false specifies that return statements expanded from macros are not checked. The default value is true.

值为 false 表示不会检查由宏展开的 return 语句。默认值为 true。
:::

::: option
StrictMode

The value false specifies that a direct return statement shall be excluded from the analysis if it is the only statement not contained in a block, like if (cond) return g();. The default value is true.

值为 false 表示如果 return 语句是唯一不在代码块中的语句（例如 if (cond) return g();），则不进行分析。默认值为 true。
:::
