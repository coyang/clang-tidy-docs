# abseil-duration-comparison

Checks for comparisons which should be in the absl::Duration domain instead of the floating point or integer domains.

检查应在 absl::Duration 域中进行的比较，而不是在浮点或整数域中进行的比较。

N.B.: In cases where a Duration was being converted to an integer and then compared against a floating-point value, truncation during the Duration conversion might yield a different result. In practice this is very rare, and still indicates a bug which should be fixed.

注意：在某些情况下，如果将 Duration 转换为整数后再与浮点值进行比较，转换过程中可能发生截断，从而导致结果不同。实际上这种情况非常罕见，但仍然表明存在应当修复的错误。

Examples:

示例：

```c++
// Original - Comparison in the floating point domain
// 原始写法 - 在浮点域中进行比较
double x;
absl::Duration d;
if (x < absl::ToDoubleSeconds(d)) ...

// Suggested - Compare in the absl::Duration domain instead
// 建议写法 - 在 absl::Duration 域中进行比较
if (absl::Seconds(x) < d) ...


// Original - Comparison in the integer domain
// 原始写法 - 在整数域中进行比较
int x;
absl::Duration d;
if (x < absl::ToInt64Microseconds(d)) ...

// Suggested - Compare in the absl::Duration domain instead
// 建议写法 - 在 absl::Duration 域中进行比较
if (absl::Microseconds(x) < d) ...
```
