# modernize-use-std-numbers

Finds constants and function calls to math functions that can be
replaced with C++20's mathematical constants from the numbers header
and offers fix-it hints. Does not match the use of variables with that
value, and instead, offers a replacement for the definition of those
variables. Function calls that match the pattern of how the constant is
calculated are matched and replaced with the std::numbers constant.
The use of macros gets replaced with the corresponding std::numbers
constant, instead of changing the macro definition.

查找可以用 C++20 中 numbers 头文件中的数学常量替代的常量和数学函数调用，并提供修复建议。不会匹配具有该值的变量的使用，而是为这些变量的定义提供替换建议。匹配符合常量计算模式的函数调用，并将其替换为 std::numbers 中的常量。宏的使用将被替换为相应的 std::numbers 常量，而不是修改宏定义。

The following list of constants from the numbers header are supported:

以下是 numbers 头文件中支持的常量列表：

- e
- log2e
- log10e
- pi
- inv_pi
- inv_sqrtpi
- ln2
- ln10
- sqrt2
- sqrt3
- inv_sqrt3
- egamma
- phi

The list currently includes all constants as of C++20.

该列表目前包含了 C++20 中的所有数学常量。

The replacements use the type of the matched constant and can remove
explicit casts, i.e., switching between std::numbers::e,
std::numbers::e_v<float> and std::numbers::e_v<long double> where
appropriate.

替换将使用匹配常量的类型，并可移除显式类型转换，例如在适当的情况下在 std::numbers::e、std::numbers::e_v<float> 和 std::numbers::e_v<long double> 之间切换。

```c++
double sqrt(double);
double log2(double);
void sink(auto&&) {}
void floatSink(float);

#define MY_PI 3.1415926

void foo() {
    const double Pi = 3.141592653589;           // const double Pi = std::numbers::pi
    const auto Use = Pi / 2;                    // no match for Pi
    static constexpr double Euler = 2.7182818;  // static constexpr double Euler = std::numbers::e;

    log2(exp(1));                               // std::numbers::log2e;
    log2(Euler);                                // std::numbers::log2e;
    1 / sqrt(MY_PI);                            // std::numbers::inv_sqrtpi;
    sink(MY_PI);                                // sink(std::numbers::pi);
    floatSink(MY_PI);                           // floatSink(std::numbers::pi);
    floatSink(static_cast<float>(MY_PI));       // floatSink(std::numbers::pi_v<float>);
}
```

```c++
double sqrt(double);
double log2(double);
void sink(auto&&) {}
void floatSink(float);

#define MY_PI 3.1415926

void foo() {
    const double Pi = 3.141592653589;           // const double Pi = std::numbers::pi
    const auto Use = Pi / 2;                    // 不匹配 Pi
    static constexpr double Euler = 2.7182818;  // static constexpr double Euler = std::numbers::e;

    log2(exp(1));                               // std::numbers::log2e;
    log2(Euler);                                // std::numbers::log2e;
    1 / sqrt(MY_PI);                            // std::numbers::inv_sqrtpi;
    sink(MY_PI);                                // sink(std::numbers::pi);
    floatSink(MY_PI);                           // floatSink(std::numbers::pi);
    floatSink(static_cast<float>(MY_PI));       // floatSink(std::numbers::pi_v<float>);
}
```

## Options

## 选项

::: option
DiffThreshold

A floating point value that sets the detection threshold for when
literals match a constant. A literal matches a constant if
abs(literal - constant) < DiffThreshold evaluates to true. Default
is 0.001.

一个浮点值，用于设置检测字面量是否匹配常量的阈值。当 abs(literal - constant) < DiffThreshold 为 true 时，字面量被认为匹配常量。默认值为 0.001。

:::

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，可选值为 llvm 或 google。默认值为 llvm。

:::
