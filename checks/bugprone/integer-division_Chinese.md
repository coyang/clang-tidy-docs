# bugprone-integer-division

Finds cases where integer division in a floating point context is likely  
to cause unintended loss of precision.

查找在浮点上下文中使用整数除法的情况，这可能会导致非预期的精度损失。

No reports are made if divisions are part of the following expressions:

- operands of operators expecting integral or bool types,
- call expressions of integral or bool types, and
- explicit cast expressions to integral or bool types,

as these are interpreted as signs of deliberateness from the programmer.

如果除法出现在以下表达式中，则不会报告问题：

- 作为期望整数或布尔类型的运算符的操作数，
- 整数或布尔类型的函数调用表达式，以及
- 显式转换为整数或布尔类型的表达式，

因为这些被视为程序员有意为之的迹象。

Examples:

示例：

```c++
float floatFunc(float);
int intFunc(int);
double d;
int i = 42;

// Warn, floating-point values expected.
d = 32 * 8 / (2 + i);
d = 8 * floatFunc(1 + 7 / 2);
d = i / (1 << 4);

// OK, no integer division.
d = 32 * 8.0 / (2 + i);
d = 8 * floatFunc(1 + 7.0 / 2);
d = (double)i / (1 << 4);

// OK, there are signs of deliberateness.
d = 1 << (i / 2);
d = 9 + intFunc(6 * i / 32);
d = (int)(i / 32) - 8;
```

```c++
// 警告，预期为浮点值。
d = 32 * 8 / (2 + i);
d = 8 * floatFunc(1 + 7 / 2);
d = i / (1 << 4);

// 正确，没有整数除法。
d = 32 * 8.0 / (2 + i);
d = 8 * floatFunc(1 + 7.0 / 2);
d = (double)i / (1 << 4);

// 正确，有明确的有意为之的迹象。
d = 1 << (i / 2);
d = 9 + intFunc(6 * i / 32);
d = (int)(i / 32) - 8;
```
