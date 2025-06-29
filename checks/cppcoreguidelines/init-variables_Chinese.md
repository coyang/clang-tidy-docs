# cppcoreguidelines-init-variables

Checks whether there are local variables that are declared without an initial value. These may lead to unexpected behavior if there is a code path that reads the variable before assigning to it.

检查是否存在未初始化的局部变量。如果在赋值前读取这些变量，可能会导致意外行为。

This rule is part of the Type safety (Type.5) profile and ES.20 from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的类型安全（Type.5）配置文件和 ES.20。

Only integers, booleans, floats, doubles and pointers are checked. The fix option initializes all detected values with the value of zero. An exception is float and double types, which are initialized to NaN.

仅检查整数、布尔值、浮点数、双精度浮点数和指针类型。修复选项会将所有检测到的变量初始化为零。浮点数和双精度浮点数类型除外，它们会被初始化为 NaN。

As an example a function that looks like this:

例如，以下函数：

```c++
void function() {
  int x;
  char *txt;
  double d;

  // Rest of the function.
}
```

Would be rewritten to look like this:

将被重写为如下形式：

```c++
#include <math.h>

void function() {
  int x = 0;
  char *txt = nullptr;
  double d = NAN;

  // Rest of the function.
}
```

It warns for the uninitialized enum case, but without a FixIt:

对于未初始化的枚举类型变量会发出警告，但不会提供自动修复：

```c++
enum A {A1, A2, A3};
enum A_c : char { A_c1, A_c2, A_c3 };
enum class B { B1, B2, B3 };
enum class B_i : int { B_i1, B_i2, B_i3 };
void function() {
  A a;     // Warning: variable 'a' is not initialized
  A_c a_c; // Warning: variable 'a_c' is not initialized
  B b;     // Warning: variable 'b' is not initialized
  B_i b_i; // Warning: variable 'b_i' is not initialized
}
```

A a; // 警告：变量 'a' 未初始化  
A_c a_c; // 警告：变量 'a_c' 未初始化  
B b; // 警告：变量 'b' 未初始化  
B_i b_i; // 警告：变量 'b_i' 未初始化

## Options

## 选项

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，可选值为 llvm 或 google。默认值为 llvm。
:::

::: option
MathHeader

A string specifying the header to include to get the definition of NAN. Default is <math.h>.

一个字符串，用于指定包含 NAN 定义所需的头文件。默认值为 <math.h>。
:::
