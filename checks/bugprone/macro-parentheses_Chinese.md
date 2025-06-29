# bugprone-macro-parentheses

Finds macros that can have unexpected behavior due to missing parentheses.

查找由于缺少括号而可能导致意外行为的宏。

Macros are expanded by the preprocessor as-is. As a result, there can be unexpected behavior; operators may be evaluated in unexpected order and unary operators may become binary operators, etc.

宏会被预处理器按原样展开。因此，可能会出现意外行为；运算符可能以意外的顺序被求值，单目运算符可能变成双目运算符等。

When the replacement list has an expression, it is recommended to surround it with parentheses. This ensures that the macro result is evaluated completely before it is used.

当替换列表中包含表达式时，建议使用括号将其括起来。这可以确保宏的结果在使用前被完整地求值。

It is also recommended to surround macro arguments in the replacement list with parentheses. This ensures that the argument value is calculated properly.

还建议在替换列表中使用括号将宏参数括起来。这可以确保参数值被正确地计算。

This check corresponds to the CERT C Coding Standard rule PRE20-C. Macro replacement lists should be parenthesized.

此检查对应于 CERT C 编码标准规则 PRE20-C：宏替换列表应加括号。

https://wiki.sei.cmu.edu/confluence/display/c/PRE02-C.+Macro+replacement+lists+should+be+parenthesized
