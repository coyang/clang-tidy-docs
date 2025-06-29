# abseil-duration-subtraction

Checks for cases where subtraction should be performed in the  
absl::Duration domain. When subtracting two values, and the first one  
is known to be a conversion from absl::Duration, we can infer that the  
second should also be interpreted as an absl::Duration, and make that  
inference explicit.

检查应在 absl::Duration 域中执行减法的情况。当减去两个值时，如果第一个值已知是从 absl::Duration 转换而来的，我们可以推断第二个值也应被解释为 absl::Duration，并明确表达这一推断。

Examples:

示例：

```c++
// Original - Subtraction in the double domain
// 原始代码 - 在 double 域中进行减法
double x;
absl::Duration d;
double result = absl::ToDoubleSeconds(d) - x;

// Suggestion - Subtraction in the absl::Duration domain instead
// 建议 - 改为在 absl::Duration 域中进行减法
double result = absl::ToDoubleSeconds(d - absl::Seconds(x));

// Original - Subtraction of two Durations in the double domain
// 原始代码 - 在 double 域中对两个 Duration 进行减法
absl::Duration d1, d2;
double result = absl::ToDoubleSeconds(d1) - absl::ToDoubleSeconds(d2);

// Suggestion - Subtraction in the absl::Duration domain instead
// 建议 - 改为在 absl::Duration 域中进行减法
double result = absl::ToDoubleSeconds(d1 - d2);
```

Note: As with other clang-tidy checks, it is possible that multiple  
fixes may overlap (as in the case of nested expressions), so not all  
occurrences can be transformed in one run. In particular, this may occur  
for nested subtraction expressions. Running clang-tidy multiple times  
will find and fix these overlaps.

注意：与其他 clang-tidy 检查类似，多个修复可能会重叠（例如在嵌套表达式的情况下），因此并非所有情况都能在一次运行中完成转换。特别是，对于嵌套的减法表达式，可能会出现这种情况。多次运行 clang-tidy 将能够发现并修复这些重叠问题。
