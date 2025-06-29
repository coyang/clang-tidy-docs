# abseil-upgrade-duration-conversions

Finds calls to absl::Duration arithmetic operators and factories whose argument needs an explicit cast to continue compiling after upcoming API changes.

查找对 absl::Duration 算术运算符和工厂函数的调用，这些调用的参数需要显式类型转换，以便在即将到来的 API 更改后继续编译。

The operators _=, /=, _, and / for absl::Duration currently accept an argument of class type that is convertible to an arithmetic type. Such a call currently converts the value to an int64_t, even in a case such as std::atomic<float> that would result in lossy conversion.

目前，absl::Duration 的运算符 _=、/=、_ 和 / 接受可以转换为算术类型的类类型参数。此类调用当前会将值转换为 int64_t，即使在像 std::atomic<float> 这样的情况下也会导致精度丢失的转换。

Additionally, the absl::Duration factory functions (absl::Hours, absl::Minutes, etc) currently accept an int64_t or a floating-point type. Similar to the arithmetic operators, calls with an argument of class type that is convertible to an arithmetic type go through the int64_t path.

此外，absl::Duration 的工厂函数（如 absl::Hours、absl::Minutes 等）目前接受 int64_t 或浮点类型。与算术运算符类似，使用可转换为算术类型的类类型参数的调用将通过 int64_t 路径进行处理。

These operators and factories will be changed to only accept arithmetic types to prevent unintended behavior. After these changes are released, passing an argument of class type will no longer compile, even if the type is implicitly convertible to an arithmetic type.

这些运算符和工厂函数将被更改为仅接受算术类型，以防止意外行为。在这些更改发布之后，传递类类型参数将无法编译，即使该类型可以隐式转换为算术类型。

Here are example fixes created by this check:

以下是此检查生成的示例修复：

```c++
std::atomic<int> a;
absl::Duration d = absl::Milliseconds(a);
d *= a;
```

becomes

变为

```c++
std::atomic<int> a;
absl::Duration d = absl::Milliseconds(static_cast<int64_t>(a));
d *= static_cast<int64_t>(a);
```

Note that this check always adds a cast to int64_t in order to preserve the current behavior of user code. It is possible that this uncovers unintended behavior due to types implicitly convertible to a floating-point type.

请注意，此检查始终添加对 int64_t 的类型转换，以保留用户代码的当前行为。这可能会暴露由于类型可隐式转换为浮点类型而导致的意外行为。
