# performance-unnecessary-value-param

Flags value parameter declarations of expensive to copy types that are copied for each invocation but it would suffice to pass them by const reference.

标记那些在每次调用时都会被复制的昂贵类型的值参数声明，而实际上通过 const 引用传递就足够了。

The check is only applied to parameters of types that are expensive to copy which means they are not trivially copyable or have a non-trivial copy constructor or destructor.

该检查仅适用于复制代价高的类型的参数，这些类型要么不是可平凡复制的，要么具有非平凡的拷贝构造函数或析构函数。

To ensure that it is safe to replace the value parameter with a const reference the following heuristic is employed:

为了确保将值参数替换为 const 引用是安全的，使用了以下启发式方法：

1.  the parameter is const qualified;
1.  参数具有 const 限定；

1.  the parameter is not const, but only const methods or operators are invoked on it, or it is used as const reference or value argument in constructors or function calls.
1.  参数不是 const 的，但仅在其上调用了 const 方法或运算符，或者它作为 const 引用或值参数被用于构造函数或函数调用中。

Example:

示例：

```c++
void f(const string Value) {
  // The warning will suggest making Value a reference.
  // 警告将建议将 Value 改为引用。
}

void g(ExpensiveToCopy Value) {
  // The warning will suggest making Value a const reference.
  // 警告将建议将 Value 改为 const 引用。
  Value.ConstMethd();
  ExpensiveToCopy Copy(Value);
}
```

If the parameter is not const, only copied or assigned once and has a non-trivial move-constructor or move-assignment operator respectively the check will suggest to move it.

如果参数不是 const 的，仅被复制或赋值一次，并且具有非平凡的移动构造函数或移动赋值运算符，则该检查将建议对其进行移动操作。

Example:

示例：

```c++
void setValue(string Value) {
  Field = Value;
}
```

Will become:

将变为：

```c++
#include <utility>

void setValue(string Value) {
  Field = std::move(Value);
}
```

Because the fix-it needs to change the signature of the function, it may break builds if the function is used in multiple translation units or some codes depends on function signatures.

由于修复建议需要更改函数签名，如果该函数在多个翻译单元中被使用，或者某些代码依赖于函数签名，则可能会导致构建失败。

## Options

## 选项

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，指定使用哪种 include 风格，可选值为 llvm 或 google。默认值为 llvm。
:::

::: option
AllowedTypes

A semicolon-separated list of names of types allowed to be passed by value. Regular expressions are accepted, e.g. [Rr]ef(erence)?$ matches every type with suffix Ref, ref, Reference and reference. The default is empty. If a name in the list contains the sequence ::, it is matched against the qualified type name (i.e. namespace::Type), otherwise it is matched against only the type name (i.e. Type).

一个以分号分隔的类型名称列表，表示允许按值传递的类型。支持正则表达式，例如 [Rr]ef(erence)?$ 匹配所有以 Ref、ref、Reference 和 reference 结尾的类型。默认值为空。如果列表中的名称包含 ::，则与限定类型名（如 namespace::Type）匹配，否则仅与类型名（如 Type）匹配。
:::

::: option
IgnoreCoroutines

A boolean specifying whether the check should suggest passing parameters by reference in coroutines. Passing parameters by reference in coroutines may not be safe, please see cppcoreguidelines-avoid-reference-coroutine-parameters for more information. Default is true.

一个布尔值，指定该检查是否应在协程中建议通过引用传递参数。在协程中通过引用传递参数可能不安全，详情请参阅 cppcoreguidelines-avoid-reference-coroutine-parameters。默认值为 true。
:::
