# abseil-duration-unnecessary-conversion

Finds and fixes cases where absl::Duration values are being converted to numeric types and back again.

查找并修复将 absl::Duration 值转换为数值类型后又转换回来的情况。

Floating-point examples:

浮点数示例：

```c++
// Original - Conversion to double and back again
// 原始代码 - 转换为 double 后再转换回来
absl::Duration d1;
absl::Duration d2 = absl::Seconds(absl::ToDoubleSeconds(d1));

// Suggestion - Remove unnecessary conversions
// 建议 - 移除不必要的转换
absl::Duration d2 = d1;

// Original - Division to convert to double and back again
// 原始代码 - 通过除法转换为 double 后再转换回来
absl::Duration d2 = absl::Seconds(absl::FDivDuration(d1, absl::Seconds(1)));

// Suggestion - Remove division and conversion
// 建议 - 移除除法和转换
absl::Duration d2 = d1;
```

Integer examples:

整数示例：

```c++
// Original - Conversion to integer and back again
// 原始代码 - 转换为整数后再转换回来
absl::Duration d1;
absl::Duration d2 = absl::Hours(absl::ToInt64Hours(d1));

// Suggestion - Remove unnecessary conversions
// 建议 - 移除不必要的转换
absl::Duration d2 = d1;

// Original - Integer division followed by conversion
// 原始代码 - 整数除法后再进行转换
absl::Duration d2 = absl::Seconds(d1 / absl::Seconds(1));

// Suggestion - Remove division and conversion
// 建议 - 移除除法和转换
absl::Duration d2 = d1;
```

Unwrapping scalar operations:

展开标量操作：

```c++
// Original - Multiplication by a scalar
// 原始代码 - 与标量相乘
absl::Duration d1;
absl::Duration d2 = absl::Seconds(absl::ToInt64Seconds(d1) * 2);

// Suggestion - Remove unnecessary conversion
// 建议 - 移除不必要的转换
absl::Duration d2 = d1 * 2;
```

Note: Converting to an integer and back to an absl::Duration might be a truncating operation if the value is not aligned to the scale of conversion. In the rare case where this is the intended result, callers should use absl::Trunc to truncate explicitly.

注意：如果值未对齐到转换的时间单位，将 absl::Duration 转换为整数再转换回来可能会导致截断操作。在极少数确实需要截断的情况下，调用者应显式使用 absl::Trunc 进行截断。
