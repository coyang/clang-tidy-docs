# bugprone-assert-side-effect

Finds assert() with side effect.

查找带有副作用的 assert()。

The condition of assert() is evaluated only in debug builds so a condition with side effect can cause different behavior in debug / release builds.

assert() 的条件只在调试版本中被求值，因此带有副作用的条件可能会导致调试版本和发布版本行为不一致。

## Options

## 选项

::: option
AssertMacros

A comma-separated list of the names of assert macros to be checked.  
Default is [assert,NSAssert,NSCAssert]{.title-ref}.

要检查的断言宏名称列表，使用逗号分隔。  
默认值为 [assert,NSAssert,NSCAssert]{.title-ref}。
:::

::: option
CheckFunctionCalls

Whether to treat non-const member and non-member functions as they produce side effects.  
Disabled by default because it can increase the number of false positive warnings.

是否将非常量成员函数和非成员函数视为具有副作用。  
默认禁用，因为启用后可能会增加误报数量。
:::

::: option
IgnoredFunctions

A semicolon-separated list of the names of functions or methods to be considered as not having side-effects.  
Regular expressions are accepted, e.g. [Rr]ef(erence)?$ matches every type with suffix Ref, ref, Reference and reference.  
The default is empty. If a name in the list contains the sequence [::]{.title-ref} it is matched against the qualified type name (i.e. namespace::Type), otherwise it is matched against only the type name (i.e. Type).

一个以分号分隔的函数或方法名称列表，这些函数或方法将被视为没有副作用。  
支持正则表达式，例如 [Rr]ef(erence)?$ 匹配所有以 Ref、ref、Reference 和 reference 结尾的类型。  
默认值为空。如果列表中的名称包含 [::]{.title-ref}，则与限定类型名（如 namespace::Type）匹配；否则仅与类型名（如 Type）匹配。
:::
