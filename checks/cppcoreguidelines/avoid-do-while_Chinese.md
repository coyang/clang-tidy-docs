# cppcoreguidelines-avoid-do-while

Warns when using do-while loops. They are less readable than plain while loops, since the termination condition is at the end and the condition is not checked prior to the first iteration. This can lead to subtle bugs.

当使用 do-while 循环时会发出警告。与普通的 while 循环相比，do-while 循环的可读性较差，因为终止条件位于循环末尾，并且在第一次迭代之前不会检查条件。这可能会导致一些隐蔽的错误。

This check implements ES.75 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 ES.75 条款。

Examples:

示例：

```c++
int x;
do {
    std::cin >> x;
    // ...
} while (x < 0);
```

## Options

## 选项

::: option
IgnoreMacros

Ignore the check when analyzing macros. This is useful for safely defining function-like macros:

在分析宏时忽略此检查。这对于安全地定义类似函数的宏非常有用：

```c++
#define FOO_BAR(x) \
do { \
  foo(x); \
  bar(x); \
} while(0)
```

Defaults to false.

默认值为 false。
:::
