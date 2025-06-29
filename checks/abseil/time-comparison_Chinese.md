# abseil-time-comparison

Prefer comparisons in the absl::Time domain instead of the integer domain.

建议在 absl::Time 域中进行比较，而不是在整数域中进行比较。

N.B.: In cases where an absl::Time is being converted to an integer, alignment may occur. If the comparison depends on this alignment, doing the comparison in the absl::Time domain may yield a different result. In practice this is very rare, and still indicates a bug which should be fixed.

注意：在将 absl::Time 转换为整数的情况下，可能会发生对齐。如果比较依赖于这种对齐，那么在 absl::Time 域中进行比较可能会产生不同的结果。实际上这种情况非常罕见，但它仍然表明存在一个应该修复的错误。

Examples:

示例：

```c++
// Original - Comparison in the integer domain
// 原始写法 - 在整数域中进行比较
int x;
absl::Time t;
if (x < absl::ToUnixSeconds(t)) ...

// Suggested - Compare in the absl::Time domain instead
// 建议写法 - 在 absl::Time 域中进行比较
if (absl::FromUnixSeconds(x) < t) ...
```
