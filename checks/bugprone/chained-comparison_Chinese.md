# bugprone-chained-comparison

Check detects chained comparison operators that can lead to unintended behavior or logical errors.

检查会检测可能导致非预期行为或逻辑错误的链式比较运算符。

Chained comparisons are expressions that use multiple comparison operators to compare three or more values. For example, the expression a < b < c compares the values of a, b, and c. However, this expression does not evaluate as (a < b) && (b < c), which is probably what the developer intended. Instead, it evaluates as (a < b) < c, which may produce unintended results, especially when the types of a, b, and c are different.

链式比较是使用多个比较运算符来比较三个或更多值的表达式。例如，表达式 a < b < c 比较了 a、b 和 c 的值。然而，该表达式并不会被计算为 (a < b) && (b < c)，尽管这可能是开发者的本意。实际上，它会被计算为 (a < b) < c，这可能会产生非预期的结果，尤其是在 a、b 和 c 的类型不同时。

To avoid such errors, the check will issue a warning when a chained comparison operator is detected, suggesting to use parentheses to specify the order of evaluation or to use a logical operator to separate comparison expressions.

为了避免此类错误，当检测到链式比较运算符时，检查器会发出警告，建议使用括号来明确计算顺序，或使用逻辑运算符来分隔比较表达式。

Consider the following examples:

请参考以下示例：

```c++
int a = 2, b = 6, c = 4;
if (a < b < c) {
    // This block will be executed
}
```

In this example, the developer intended to check if a is less than b and b is less than c. However, the expression a < b < c is equivalent to (a < b) < c. Since a < b is true, the expression (a < b) < c is evaluated as 1 < c, which is equivalent to true < c and is invalid in this case as b < c is false.

在这个例子中，开发者本意是检查 a 是否小于 b 且 b 是否小于 c。然而，表达式 a < b < c 实际上等价于 (a < b) < c。由于 a < b 为 true，该表达式被计算为 1 < c，也就是 true < c。在这种情况下是无效的，因为 b < c 实际上是 false。

Even that above issue could be detected as comparison of int to bool, there is more dangerous example:

即使上述问题可以被识别为 int 与 bool 的比较，还有一个更危险的例子：

```c++
bool a = false, b = false, c = true;
if (a == b == c) {
    // This block will be executed
}
```

In this example, the developer intended to check if a, b, and c are all equal. However, the expression a == b == c is evaluated as (a == b) == c. Since a == b is true, the expression (a == b) == c is evaluated as true == c, which is equivalent to true == true. This comparison yields true, even though a and b are false, and are not equal to c.

在这个例子中，开发者本意是检查 a、b 和 c 是否都相等。然而，表达式 a == b == c 实际上被计算为 (a == b) == c。由于 a == b 为 true，该表达式被计算为 true == c，也就是 true == true。这个比较结果为 true，尽管 a 和 b 为 false，实际上并不等于 c。

To avoid this issue, the developer can use a logical operator to separate the comparison expressions, like this:

为避免此问题，开发者可以使用逻辑运算符来分隔比较表达式，如下所示：

```c++
if (a == b && b == c) {
    // This block will not be executed
}
```

Alternatively, use of parentheses in the comparison expressions can make the developer's intention more explicit and help avoid misunderstanding.

另外，在比较表达式中使用括号可以使开发者的意图更加明确，有助于避免误解。

```c++
if ((a == b) == c) {
    // This block will be executed
}
```
