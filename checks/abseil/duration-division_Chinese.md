# abseil-duration-division

absl::Duration 的算术运算行为类似于整数运算。这意味着两个 absl::Duration 对象相除会返回一个 int64 类型的结果，并且任何小数部分都会向 0 截断。有关 absl::Duration 算术运算的更多信息，请参见此链接：  
[this link](https://github.com/abseil/abseil-cpp/blob/29ff6d4860070bf8fcbd39c8805d0c32d56628a3/absl/time/time.h#L137)

absl::Duration 的算术运算行为类似于整数运算。这意味着两个 absl::Duration 对象相除会返回一个 int64 类型的结果，并且任何小数部分都会向 0 截断。有关 absl::Duration 算术运算的更多信息，请参见此链接：  
[this link](https://github.com/abseil/abseil-cpp/blob/29ff6d4860070bf8fcbd39c8805d0c32d56628a3/absl/time/time.h#L137)

例如：

例如：

```c++
absl::Duration d = absl::Seconds(3.5);
int64 sec1 = d / absl::Seconds(1);     // Truncates toward 0.
int64 sec2 = absl::ToInt64Seconds(d);  // Equivalent to division.
assert(sec1 == 3 && sec2 == 3);

double dsec = d / absl::Seconds(1);  // WRONG: Still truncates toward 0.
assert(dsec == 3.0);
```

```c++
absl::Duration d = absl::Seconds(3.5);
int64 sec1 = d / absl::Seconds(1);     // 向 0 截断。
int64 sec2 = absl::ToInt64Seconds(d);  // 等价于除法。
assert(sec1 == 3 && sec2 == 3);

double dsec = d / absl::Seconds(1);  // 错误：仍然向 0 截断。
assert(dsec == 3.0);
```

如果你想要进行浮点除法，应该使用 absl::FDivDuration() 函数，或使用如 absl::ToDoubleSeconds() 这样的单位转换函数。例如：

如果你想要进行浮点除法，应该使用 absl::FDivDuration() 函数，或使用如 absl::ToDoubleSeconds() 这样的单位转换函数。例如：

```c++
absl::Duration d = absl::Seconds(3.5);
double dsec1 = absl::FDivDuration(d, absl::Seconds(1));  // GOOD: No truncation.
double dsec2 = absl::ToDoubleSeconds(d);                 // GOOD: No truncation.
assert(dsec1 == 3.5 && dsec2 == 3.5);
```

```c++
absl::Duration d = absl::Seconds(3.5);
double dsec1 = absl::FDivDuration(d, absl::Seconds(1));  // 正确：没有截断。
double dsec2 = absl::ToDoubleSeconds(d);                 // 正确：没有截断。
assert(dsec1 == 3.5 && dsec2 == 3.5);
```

此检查会查找在浮点上下文中对 absl::Duration 进行除法运算的用法，并建议使用返回浮点值的函数。

此检查会查找在浮点上下文中对 absl::Duration 进行除法运算的用法，并建议使用返回浮点值的函数。
