# cppcoreguidelines-pro-type-static-cast-downcast

This check flags all usages of static_cast, where a base class is casted to a derived class. In those cases, a fix-it is provided to convert the cast to a dynamic_cast.

此检查会标记所有使用 static_cast 的情况，其中基类被转换为派生类。在这些情况下，会提供一个修复建议，将该转换改为 dynamic_cast。

Use of these casts can violate type safety and cause the program to access a variable that is actually of type X to be accessed as if it were of an unrelated type Z.

使用这些类型转换可能会违反类型安全，导致程序将实际类型为 X 的变量当作无关类型 Z 来访问。

This rule is part of the Type safety (Type.2) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的类型安全（Type.2）配置文件的一部分。

链接: https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Pro-type-downcast

## Options

## 选项

::: option
StrictMode

When set to false, no warnings are emitted for casts on non-polymorphic types. Default is true.

当设置为 false 时，对于非多态类型的转换将不会发出警告。默认值为 true。
:::
