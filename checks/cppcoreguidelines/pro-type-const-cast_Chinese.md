# cppcoreguidelines-pro-type-const-cast

Imposes limitations on the use of const_cast within C++ code. It depends on the StrictMode option setting to determine whether it should flag all instances of const_cast or only those that remove either const or volatile qualifier.

对 C++ 代码中 const_cast 的使用施加限制。该规则依赖于 StrictMode 选项设置，以决定是否应标记所有 const_cast 的使用，还是仅标记那些移除了 const 或 volatile 限定符的情况。

Modifying a variable that has been declared as const in C++ is generally considered undefined behavior, and this remains true even when using const_cast. In C++, the const qualifier indicates that a variable is intended to be read-only, and the compiler enforces this by disallowing any attempts to change the value of that variable.

修改一个在 C++ 中被声明为 const 的变量通常被认为是未定义行为，即使使用 const_cast 也不例外。在 C++ 中，const 限定符表示变量是只读的，编译器通过禁止任何更改该变量值的尝试来强制执行这一点。

Removing the volatile qualifier in C++ can have serious consequences. This qualifier indicates that a variable's value can change unpredictably, and removing it may lead to undefined behavior, optimization problems, and debugging challenges. It's essential to retain the volatile qualifier in situations where the variable's volatility is a crucial aspect of program correctness and reliability.

在 C++ 中移除 volatile 限定符可能会带来严重后果。该限定符表示变量的值可能会不可预测地发生变化，移除它可能导致未定义行为、优化问题以及调试困难。在变量的易变性对程序的正确性和可靠性至关重要的情况下，保留 volatile 限定符是非常必要的。

This rule is part of the Type safety (Type 3) profile and ES.50: Don't cast away const rule from the C++ Core Guidelines.

该规则属于 C++ 核心指南中的类型安全（类型 3）配置文件以及 ES.50：不要移除 const 的规则。

## Options

选项

::: option
StrictMode

StrictMode

When this setting is set to true, it means that any usage of const_cast is not allowed. On the other hand, when it's set to false, it permits casting to const or volatile types. Default value is false.

当该设置为 true 时，表示不允许任何 const_cast 的使用。相反，当其设置为 false 时，允许转换为 const 或 volatile 类型。默认值为 false。
:::
