# readability-inconsistent-declaration-parameter-name

Find function declarations which differ in parameter names.

查找参数名称不一致的函数声明。

Example:

示例：

```c++
// in foo.hpp:
void foo(int a, int b, int c);

// in foo.cpp:
void foo(int d, int e, int f); // warning
```

This check should help to enforce consistency in large projects, where it often happens that a definition of function is refactored, changing the parameter names, but its declaration in header file is not updated. With this check, we can easily find and correct such inconsistencies, keeping declaration and definition always in sync.

此检查有助于在大型项目中保持一致性。在这些项目中，函数定义经常被重构，参数名称发生变化，但头文件中的声明未被更新。通过此检查，我们可以轻松发现并纠正此类不一致，使声明和定义始终保持同步。

Unnamed parameters are allowed and are not taken into account when comparing function declarations, for example:

允许未命名参数，在比较函数声明时不会考虑这些参数，例如：

```c++
void foo(int a);
void foo(int); // no warning
```

One name is also allowed to be a case-insensitive prefix/suffix of the other:

一个名称也可以是另一个名称的不区分大小写的前缀或后缀：

```c++
void foo(int count);
void foo(int count_input) { // no warning
  int count = adjustCount(count_input);
}
```

To help with refactoring, in some cases fix-it hints are generated to align parameter names to a single naming convention. This works with the assumption that the function definition is the most up-to-date version, as it directly references parameter names in its body. Example:

为了帮助重构，在某些情况下会生成修复建议（fix-it hints），以将参数名称统一为一种命名规范。这是基于函数定义是最新版本的假设，因为定义体中直接引用了参数名称。示例：

```c++
void foo(int a); // warning and fix-it hint (replace "a" to "b")
int foo(int b) { return b + 2; } // definition with use of "b"
```

In the case of multiple redeclarations or function template specializations, a warning is issued for every redeclaration or specialization inconsistent with the definition or the first declaration seen in a translation unit.

在存在多个重新声明或函数模板特化的情况下，凡是与定义或翻译单元中首次出现的声明不一致的重新声明或特化，都会触发警告。

## Options

选项

::: option
IgnoreMacros

If this option is set to true (default is true), the check will not warn about names declared inside macros.

如果此选项设置为 true（默认值为 true），则不会对宏中声明的名称发出警告。
:::

::: option
Strict

If this option is set to true (default is false), then names must match exactly (or be absent).

如果此选项设置为 true（默认值为 false），则参数名称必须完全匹配（或均未命名）。
:::
