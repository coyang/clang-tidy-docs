# cppcoreguidelines-pro-type-cstyle-cast

This check flags all use of C-style casts that perform a static_cast downcast, const_cast, or reinterpret_cast.

此检查会标记所有使用 C 风格强制类型转换的情况，这些转换执行了 static_cast 向下转换、const_cast 或 reinterpret_cast。

Use of these casts can violate type safety and cause the program to access a variable that is actually of type X to be accessed as if it were of an unrelated type Z. Note that a C-style (T)expression cast means to perform the first of the following that is possible: a const_cast, a static_cast, a static_cast followed by a const_cast, a reinterpret_cast, or a reinterpret_cast followed by a const_cast. This rule bans (T)expression only when used to perform an unsafe cast.

使用这些类型转换可能会违反类型安全，并导致程序以不相关的类型 Z 来访问实际为类型 X 的变量。请注意，C 风格的 (T)expression 类型转换表示执行以下操作中第一个可行的：const_cast、static_cast、static_cast 后跟 const_cast、reinterpret_cast，或 reinterpret_cast 后跟 const_cast。此规则仅在 (T)expression 用于执行不安全的类型转换时才禁止使用。

This rule is part of the Type safety (Type.4) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的类型安全（Type.4）配置文件的一部分。
链接: https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Pro-type-cstylecast
