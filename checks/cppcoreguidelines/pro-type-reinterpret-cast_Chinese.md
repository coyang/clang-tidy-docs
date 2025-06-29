# cppcoreguidelines-pro-type-reinterpret-cast

This check flags all uses of reinterpret_cast in C++ code.

此检查会标记 C++ 代码中所有使用 reinterpret_cast 的情况。

Use of these casts can violate type safety and cause the program to access a variable that is actually of type X to be accessed as if it were of an unrelated type Z.

使用这种类型转换可能会违反类型安全，导致程序将实际为类型 X 的变量当作无关的类型 Z 来访问。

This rule is part of the Type safety (Type.1.1) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的类型安全（Type.1.1）规范的一部分。

链接: [Type safety (Type.1.1)](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Pro-type-reinterpretcast)
