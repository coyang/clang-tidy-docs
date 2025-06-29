# readability-avoid-nested-conditional-operator

Identifies instances of nested conditional operators in the code.

识别代码中嵌套条件运算符的情况。

Nested conditional operators, also known as ternary operators, can contribute to reduced code readability and comprehension. So they should be split as several statements and stored the intermediate results in temporary variable.

嵌套条件运算符（也称为三元运算符）可能会降低代码的可读性和可理解性。因此，应该将其拆分为多个语句，并将中间结果存储在临时变量中。

Examples:

示例：

```c++
int NestInConditional = (condition1 ? true1 : false1) ? true2 : false2;
int NestInTrue = condition1 ? (condition2 ? true1 : false1) : false2;
int NestInFalse = condition1 ? true1 : condition2 ? true2 : false1;
```
