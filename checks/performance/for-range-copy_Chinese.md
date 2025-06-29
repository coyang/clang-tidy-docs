# performance-for-range-copy

Finds C++11 for ranges where the loop variable is copied in each iteration but it would suffice to obtain it by const reference.

查找 C++11 范围 for 循环中，每次迭代都会复制循环变量的情况，而实际上使用 const 引用即可满足需求。

The check is only applied to loop variables of types that are expensive to copy which means they are not trivially copyable or have a non-trivial copy constructor or destructor.

该检查仅适用于复制开销较大的类型的循环变量，即这些类型不是平凡可复制的，或具有非平凡的拷贝构造函数或析构函数。

To ensure that it is safe to replace the copy with a const reference the following heuristic is employed:

为了确保将复制替换为 const 引用是安全的，使用了以下启发式方法：

1.  The loop variable is const qualified.  
    循环变量被声明为 const。

2.  The loop variable is not const, but only const methods or operators are invoked on it, or it is used as const reference or value argument in constructors or function calls.  
    循环变量不是 const，但仅调用其 const 方法或运算符，或者在构造函数或函数调用中作为 const 引用或值参数使用。

## Options

## 选项

::: option
WarnOnAllAutoCopies

When true, warns on any use of auto as the type of the range-based for loop variable. Default is false.

当设置为 true 时，会对范围 for 循环中使用 auto 作为循环变量类型的任何情况发出警告。默认值为 false。
:::

::: option
AllowedTypes

A semicolon-separated list of names of types allowed to be copied in each iteration. Regular expressions are accepted, e.g. [Rr]ef(erence)?$ matches every type with suffix Ref, ref, Reference and reference. The default is empty. If a name in the list contains the sequence ::, it is matched against the qualified type name (i.e. namespace::Type), otherwise it is matched against only the type name (i.e. Type).

一个以分号分隔的类型名称列表，表示允许在每次迭代中复制的类型。支持正则表达式，例如 [Rr]ef(erence)?$ 匹配所有以 Ref、ref、Reference 和 reference 结尾的类型。默认值为空。如果列表中的名称包含 ::，则与限定类型名（例如 namespace::Type）进行匹配；否则，仅与类型名（例如 Type）进行匹配。
:::
