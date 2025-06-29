# abseil-duration-conversion-cast

Checks for casts of absl::Duration conversion functions, and recommends the right conversion function instead.

检查对 absl::Duration 转换函数的类型转换，并推荐使用正确的转换函数。

Examples:

示例：

```c++
// Original - Cast from a double to an integer
// 原始写法 - 从 double 类型转换为整数类型
absl::Duration d;
int i = static_cast<int>(absl::ToDoubleSeconds(d));

// Suggested - Use the integer conversion function directly.
// 建议写法 - 直接使用整数类型的转换函数
int i = absl::ToInt64Seconds(d);


// Original - Cast from a double to an integer
// 原始写法 - 从整数类型转换为 double 类型
absl::Duration d;
double x = static_cast<double>(absl::ToInt64Seconds(d));

// Suggested - Use the integer conversion function directly.
// 建议写法 - 直接使用 double 类型的转换函数
double x = absl::ToDoubleSeconds(d);
```

Note: In the second example, the suggested fix could yield a different result, as the conversion to integer could truncate. In practice, this is very rare, and you should use absl::Trunc to perform this operation explicitly instead.

注意：在第二个示例中，建议的修复方式可能会产生不同的结果，因为转换为整数时可能会发生截断。在实际应用中，这种情况非常罕见，您应当使用 absl::Trunc 来显式地执行该操作。
