# abseil-duration-factory-float

Checks for cases where the floating-point overloads of various  
absl::Duration factory functions are called when the more-efficient  
integer versions could be used instead.

检查在调用 absl::Duration 各种工厂函数的浮点数重载版本时，是否可以改用更高效的整数版本。

This check will not suggest fixes for literals which contain fractional  
floating point values or non-literals. It will suggest removing  
superfluous casts.

该检查不会对包含小数部分的浮点字面量或非常量值提出修改建议。它会建议移除多余的类型转换。

Examples:  
示例：

```c++
// Original - Providing a floating-point literal.
// 原始写法 - 使用浮点字面量。
absl::Duration d = absl::Seconds(10.0);

// Suggested - Use an integer instead.
// 建议写法 - 改用整数。
absl::Duration d = absl::Seconds(10);


// Original - Explicitly casting to a floating-point type.
// 原始写法 - 显式转换为浮点类型。
absl::Duration d = absl::Seconds(static_cast<double>(10));

// Suggested - Remove the explicit cast
// 建议写法 - 移除显式类型转换。
absl::Duration d = absl::Seconds(10);
```
