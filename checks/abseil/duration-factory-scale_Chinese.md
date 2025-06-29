# abseil-duration-factory-scale

Checks for cases where arguments to absl::Duration factory functions are scaled internally and could be changed to a different factory function. This check also looks for arguments with a zero value and suggests using absl::ZeroDuration() instead.

检查 absl::Duration 工厂函数的参数在内部被缩放的情况，并建议改用更合适的工厂函数。此检查还会查找值为零的参数，并建议使用 absl::ZeroDuration() 代替。

Examples:

示例：

```c++
// Original - Internal multiplication.
int x;
absl::Duration d = absl::Seconds(60 * x);

// Suggested - Use absl::Minutes instead.
absl::Duration d = absl::Minutes(x);

// 原始写法 - 内部乘法。
int x;
absl::Duration d = absl::Seconds(60 * x);

// 建议写法 - 使用 absl::Minutes 替代。
absl::Duration d = absl::Minutes(x);


// Original - Internal division.
int y;
absl::Duration d = absl::Milliseconds(y / 1000.);

// Suggested - Use absl::Seconds instead.
absl::Duration d = absl::Seconds(y);

// 原始写法 - 内部除法。
int y;
absl::Duration d = absl::Milliseconds(y / 1000.);

// 建议写法 - 使用 absl::Seconds 替代。
absl::Duration d = absl::Seconds(y);


// Original - Zero-value argument.
absl::Duration d = absl::Hours(0);

// Suggested = Use absl::ZeroDuration instead
absl::Duration d = absl::ZeroDuration();

// 原始写法 - 参数为零值。
absl::Duration d = absl::Hours(0);

// 建议写法 - 使用 absl::ZeroDuration 替代。
absl::Duration d = absl::ZeroDuration();
```
