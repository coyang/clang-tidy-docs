# cppcoreguidelines-pro-bounds-constant-array-index

This check flags all array subscript expressions on static arrays and
std::arrays that either do not have a constant integer expression
index or are out of bounds (for std::array). For out-of-bounds
checking of static arrays, see the -Warray-bounds Clang
diagnostic.

该检查会标记所有对静态数组和 std::array 的下标表达式，这些表达式要么没有使用常量整数表达式作为索引，要么（对于 std::array）索引越界。对于静态数组的越界检查，请参见 Clang 的 -Warray-bounds 诊断。

This rule is part of the Bounds safety (Bounds 2) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的“边界安全性（Bounds 2）”配置文件的一部分。

Optionally, this check can generate fixes using gsl::at for indexing.

此检查还可以选择使用 gsl::at 进行索引访问，并自动生成修复建议。

## Options

## 选项

::: option
GslHeader

The check can generate fixes after this option has been set to the name
of the include file that contains gsl::at(), e.g.
"gsl/gsl.h". Default is an empty string.

设置此选项为包含 gsl::at() 的头文件名称（例如 "gsl/gsl.h"）后，该检查可以生成修复建议。默认值为空字符串。

:::

::: option
IncludeStyle

A string specifying which include-style is used, llvm or
google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，可选值为 llvm 或 google。默认值为 llvm。

:::
