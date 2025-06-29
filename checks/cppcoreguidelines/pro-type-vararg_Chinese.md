# cppcoreguidelines-pro-type-vararg

This check flags all calls to c-style vararg functions and all use of va_arg.

此检查会标记所有对 C 风格可变参数函数的调用以及所有对 va_arg 的使用。

To allow for SFINAE use of vararg functions, a call is not flagged if a literal 0 is passed as the only vararg argument or function is used in unevaluated context.

为了允许在 SFINAE 中使用可变参数函数，如果仅传递字面值 0 作为唯一的可变参数，或者函数在未求值上下文中使用，则不会被标记。

Passing to varargs assumes the correct type will be read. This is fragile because it cannot generally be enforced to be safe in the language and so relies on programmer discipline to get it right.

传递参数给可变参数函数时假设会读取正确的类型。这种做法很脆弱，因为在语言层面通常无法强制保证其安全，因此依赖程序员的自律来确保正确性。

This rule is part of the Type safety (Type.8) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的类型安全（Type.8）规范。
