# abseil-duration-addition

Check for cases where addition should be performed in the absl::Time domain. When adding two values, and one is known to be an absl::Time, we can infer that the other should be interpreted as an absl::Duration of a similar scale, and make that inference explicit.

检查应在 absl::Time 域中执行加法的情况。当两个值相加，其中一个已知是 absl::Time 类型时，我们可以推断另一个值应被解释为具有相似时间尺度的 absl::Duration，并将这种推断显式化。

Examples:

示例：

```c++
// Original - Addition in the integer domain
// 原始代码 - 在整数域中进行加法
int x;
absl::Time t;
int result = absl::ToUnixSeconds(t) + x;

// Suggestion - Addition in the absl::Time domain
// 建议修改 - 在 absl::Time 域中进行加法
int result = absl::ToUnixSeconds(t + absl::Seconds(x));
```
