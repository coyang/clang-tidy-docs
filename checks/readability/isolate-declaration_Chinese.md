以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔：

# readability-isolate-declaration

Detects local variable declarations declaring more than one variable and
tries to refactor the code to one statement per declaration.

检测声明多个局部变量的声明语句，并尝试将代码重构为每个声明只包含一个变量。

The automatic code-transformation will use the same indentation as the
original for every created statement and add a line break after each
statement. It keeps the order of the variable declarations consistent,
too.

自动代码转换将为每个新创建的语句使用与原始语句相同的缩进，并在每个语句后添加换行符。同时，它也会保持变量声明的顺序一致。

```c++
void f() {
  int * pointer = nullptr, value = 42, * const const_ptr = &value;
  // This declaration will be diagnosed and transformed into:
  // int * pointer = nullptr;
  // int value = 42;
  // int * const const_ptr = &value;
}
```

```c++
void f() {
  int * pointer = nullptr, value = 42, * const const_ptr = &value;
  // 此声明将被诊断并转换为：
  // int * pointer = nullptr;
  // int value = 42;
  // int * const const_ptr = &value;
}
```

The check excludes places where it is necessary or common to declare
multiple variables in one statement and there is no other way supported
in the language. Please note that structured bindings are not
considered.

该检查排除了一些必须或常见地在一个语句中声明多个变量的情况，并且语言本身不支持其他方式。请注意，结构化绑定不在考虑范围内。

```c++
// It is not possible to transform this declaration and doing the declaration
// before the loop will increase the scope of the variable 'Begin' and 'End'
// which is undesirable.
for (int Begin = 0, End = 100; Begin < End; ++Begin);
if (int Begin = 42, Result = some_function(Begin); Begin == Result);

// It is not possible to transform this declaration because the result is
// not functionality preserving as 'j' and 'k' would not be part of the
// 'if' statement anymore.
if (SomeCondition())
  int i = 42, j = 43, k = function(i,j);
```

```c++
// 无法转换此声明，因为在循环前声明会扩大变量 'Begin' 和 'End' 的作用域，
// 这是不希望发生的。
for (int Begin = 0, End = 100; Begin < End; ++Begin);
if (int Begin = 42, Result = some_function(Begin); Begin == Result);

// 无法转换此声明，因为转换后的结果无法保持原有功能，
// 因为 'j' 和 'k' 将不再属于 'if' 语句的一部分。
if (SomeCondition())
  int i = 42, j = 43, k = function(i,j);
```

## Limitations

限制

Global variables and member variables are excluded.

全局变量和成员变量不在检查范围内。

The check currently does not support the automatic transformation of
member-pointer-types.

该检查目前不支持成员指针类型的自动转换。

```c++
struct S {
  int a;
  const int b;
  void f() {}
};

void f() {
  // Only a diagnostic message is emitted
  int S::*p = &S::a, S::*const q = &S::a;
}
```

```c++
struct S {
  int a;
  const int b;
  void f() {}
};

void f() {
  // 仅会发出诊断信息
  int S::*p = &S::a, S::*const q = &S::a;
}
```

Furthermore, the transformation is very cautious when it detects various
kinds of macros or preprocessor directives in the range of the
statement. In this case the transformation will not happen to avoid
unexpected side-effects due to macros.

此外，当检测到语句范围内存在各种宏或预处理指令时，转换将非常谨慎。在这种情况下，为避免由于宏引起的意外副作用，将不会进行转换。

```c++
#define NULL 0
#define MY_NICE_TYPE int **
#define VAR_NAME(name) name##__LINE__
#define A_BUNCH_OF_VARIABLES int m1 = 42, m2 = 43, m3 = 44;

void macros() {
  int *p1 = NULL, *p2 = NULL;
  // Will be transformed to
  // int *p1 = NULL;
  // int *p2 = NULL;

  MY_NICE_TYPE p3, v1, v2;
  // Won't be transformed, but a diagnostic is emitted.

  int VAR_NAME(v3),
      VAR_NAME(v4),
      VAR_NAME(v5);
  // Won't be transformed, but a diagnostic is emitted.

  A_BUNCH_OF_VARIABLES
  // Won't be transformed, but a diagnostic is emitted.

  int Unconditional,
#if CONFIGURATION
      IfConfigured = 42,
#else
      IfConfigured = 0;
#endif
  // Won't be transformed, but a diagnostic is emitted.
}
```

```c++
#define NULL 0
#define MY_NICE_TYPE int **
#define VAR_NAME(name) name##__LINE__
#define A_BUNCH_OF_VARIABLES int m1 = 42, m2 = 43, m3 = 44;

void macros() {
  int *p1 = NULL, *p2 = NULL;
  // 将被转换为
  // int *p1 = NULL;
  // int *p2 = NULL;

  MY_NICE_TYPE p3, v1, v2;
  // 不会被转换，但会发出诊断信息。

  int VAR_NAME(v3),
      VAR_NAME(v4),
      VAR_NAME(v5);
  // 不会被转换，但会发出诊断信息。

  A_BUNCH_OF_VARIABLES
  // 不会被转换，但会发出诊断信息。

  int Unconditional,
#if CONFIGURATION
      IfConfigured = 42,
#else
      IfConfigured = 0;
#endif
  // 不会被转换，但会发出诊断信息。
}
```
