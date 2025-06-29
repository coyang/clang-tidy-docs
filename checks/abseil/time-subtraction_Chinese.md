# abseil-time-subtraction

Finds and fixes absl::Time subtraction expressions to do subtraction in the Time domain instead of the numeric domain.

查找并修复 absl::Time 的减法表达式，以在时间域中进行减法运算，而不是在数值域中。

There are two cases of Time subtraction in which deduce additional type information:

在以下两种时间减法的情况下，可以推断出额外的类型信息：

- When the result is an absl::Duration and the first argument is an absl::Time.
- 当结果是 absl::Duration 且第一个参数是 absl::Time 时。
- When the second argument is a absl::Time.
- 当第二个参数是 absl::Time 时。

In the first case, we must know the result of the operation, since without that the second operand could be either an absl::Time or an absl::Duration. In the second case, the first operand must be an absl::Time, because subtracting an absl::Time from an absl::Duration is not defined.

在第一种情况下，我们必须知道操作的结果类型，因为如果不知道，第二个操作数可能是 absl::Time 或 absl::Duration。 在第二种情况下，第一个操作数必须是 absl::Time，因为从 absl::Duration 中减去 absl::Time 是未定义的行为。

Examples:

示例：

```c++
int x;
absl::Time t;

// Original - absl::Duration result and first operand is an absl::Time.
// 原始写法 - 结果为 absl::Duration，且第一个操作数是 absl::Time。
absl::Duration d = absl::Seconds(absl::ToUnixSeconds(t) - x);

// Suggestion - Perform subtraction in the Time domain instead.
// 建议写法 - 在时间域中进行减法运算。
absl::Duration d = t - absl::FromUnixSeconds(x);


// Original - Second operand is an absl::Time.
// 原始写法 - 第二个操作数是 absl::Time。
int i = x - absl::ToUnixSeconds(t);

// Suggestion - Perform subtraction in the Time domain instead.
// 建议写法 - 在时间域中进行减法运算。
int i = absl::ToInt64Seconds(absl::FromUnixSeconds(x) - t);
```
