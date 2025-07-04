# readability-avoid-nested-conditional-operator

Identifies instances of nested conditional operators in the code.

Nested conditional operators, also known as ternary operators, can
contribute to reduced code readability and comprehension. So they should
be split as several statements and stored the intermediate results in
temporary variable.

Examples:

```c++
int NestInConditional = (condition1 ? true1 : false1) ? true2 : false2;
int NestInTrue = condition1 ? (condition2 ? true1 : false1) : false2;
int NestInFalse = condition1 ? true1 : condition2 ? true2 : false1;
```
