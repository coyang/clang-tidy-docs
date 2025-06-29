# modernize-use-integer-sign-comparison

Replace comparisons between signed and unsigned integers with their safe C++20 std::cmp\_\* alternative, if available.

将有符号整数与无符号整数之间的比较替换为 C++20 中安全的 std::cmp\_\* 替代函数（如果可用）。

The check provides a replacement only for C++20 or later, otherwise it highlights the problem and expects the user to fix it manually.

该检查仅在 C++20 或更高版本中提供替换，否则它会标出问题，并期望用户手动修复。

Examples of fixes created by the check:

该检查生成的修复示例如下：

```c++
unsigned int func(int a, unsigned int b) {
  return a == b;
}
```

becomes

变为

```c++
#include <utility>

unsigned int func(int a, unsigned int b) {
  return std::cmp_equal(a, b);
}
```

## Options

## 选项

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，llvm 或 google。默认值为 llvm。
:::

::: option
EnableQtSupport

Makes C++17 q20::cmp\_\* alternative available for Qt-based applications. Default is false.

为基于 Qt 的应用程序启用 C++17 中的 q20::cmp\_\* 替代函数。默认值为 false。
:::
