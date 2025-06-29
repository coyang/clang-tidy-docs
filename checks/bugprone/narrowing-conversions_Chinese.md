# bugprone-narrowing-conversions

[cppcoreguidelines-narrowing-conversions]{.title-ref} 是此检查项的别名。

[cppcoreguidelines-narrowing-conversions]{.title-ref} 是此检查项的别名。

Checks for silent narrowing conversions, e.g: int i = 0; i += 0.1;.
While the issue is obvious in this former example, it might not be so in
the following: void MyClass::f(double d) { int*member* += d; }.

检查静默的缩窄转换，例如：int i = 0; i += 0.1;。
虽然在前一个示例中问题很明显，但在如下代码中可能就不那么明显：
void MyClass::f(double d) { int*member* += d; }。

We flag narrowing conversions from:

我们会标记以下类型的缩窄转换：

- an integer to a narrower integer (e.g. char to unsigned char) if WarnOnIntegerNarrowingConversion Option is set,
- an integer to a narrower floating-point (e.g. uint64_t to float) if WarnOnIntegerToFloatingPointNarrowingConversion Option is set,
- a floating-point to an integer (e.g. double to int),
- a floating-point to a narrower floating-point (e.g. double to float) if WarnOnFloatingPointNarrowingConversion Option is set.

- 从整数到更窄的整数（例如 char 到 unsigned char），如果启用了 WarnOnIntegerNarrowingConversion 选项；
- 从整数到更窄的浮点数（例如 uint64_t 到 float），如果启用了 WarnOnIntegerToFloatingPointNarrowingConversion 选项；
- 从浮点数到整数（例如 double 到 int）；
- 从浮点数到更窄的浮点数（例如 double 到 float），如果启用了 WarnOnFloatingPointNarrowingConversion 选项。

This check will flag:

此检查项会标记以下情况：

- All narrowing conversions that are not marked by an explicit cast (c-style or static_cast). For example: int i = 0; i += 0.1;, void f(int); f(0.1);
- All applications of binary operators with a narrowing conversions. For example: int i; i+= 0.1;

- 所有未使用显式类型转换（C 风格或 static_cast）标记的缩窄转换。例如：int i = 0; i += 0.1;，void f(int); f(0.1);
- 所有涉及缩窄转换的二元运算符应用。例如：int i; i += 0.1;

Arithmetic with smaller integer types than int trigger implicit conversions, as explained under "Integral Promotion" on cppreference.com.
This check diagnoses more instances of narrowing than the compiler warning -Wconversion does. The example below demonstrates this behavior.

当使用比 int 更小的整数类型进行算术运算时，会触发隐式转换，详见 cppreference.com 上的 “Integral Promotion”。
此检查项诊断的缩窄转换情况比编译器警告 -Wconversion 更多。以下示例展示了这种行为。

```c++
// The following function definition demonstrates usage of arithmetic with
// integer types smaller than `int` and how the narrowing conversion happens
// implicitly.
void computation(short argument1, short argument2) {
  // Arithmetic written by humans:
  short result = argument1 + argument2;
  // Arithmetic actually performed by C++:
  short result = static_cast<short>(static_cast<int>(argument1) + static_cast<int>(argument2));
}

void recommended_resolution(short argument1, short argument2) {
  short result = argument1 + argument2;
  //           ^ warning: narrowing conversion from 'int' to signed type 'short' is implementation-defined

  // The cppcoreguidelines recommend to resolve this issue by using the GSL
  // in one of two ways. Either by a cast that throws if a loss of precision
  // would occur.
  short result = gsl::narrow<short>(argument1 + argument2);
  // Or it can be resolved without checking the result risking invalid results.
  short result = gsl::narrow_cast<short>(argument1 + argument2);

  // A classical `static_cast` will silence the warning as well if the GSL
  // is not available.
  short result = static_cast<short>(argument1 + argument2);
}
```

```c++
// 以下函数定义演示了使用比 `int` 更小的整数类型进行算术运算，
// 以及缩窄转换是如何隐式发生的。
void computation(short argument1, short argument2) {
  // 人类编写的算术运算：
  short result = argument1 + argument2;
  // C++ 实际执行的算术运算：
  short result = static_cast<short>(static_cast<int>(argument1) + static_cast<int>(argument2));
}

void recommended_resolution(short argument1, short argument2) {
  short result = argument1 + argument2;
  //           ^ 警告：从 'int' 到有符号类型 'short' 的缩窄转换是实现定义的

  // cppcoreguidelines 推荐使用 GSL 以两种方式之一解决此问题。
  // 一种方式是使用在发生精度丢失时抛出异常的转换。
  short result = gsl::narrow<short>(argument1 + argument2);
  // 或者可以在不检查结果的情况下解决，但可能导致无效结果。
  short result = gsl::narrow_cast<short>(argument1 + argument2);

  // 如果无法使用 GSL，传统的 `static_cast` 也可以消除警告。
  short result = static_cast<short>(argument1 + argument2);
}
```

## Options

## 选项

::: option
WarnOnIntegerNarrowingConversion

When true, the check will warn on narrowing integer conversion (e.g. int to size_t). true by default.

当为 true 时，此检查项会对整数的缩窄转换发出警告（例如 int 到 size_t）。默认值为 true。
:::

::: option
WarnOnIntegerToFloatingPointNarrowingConversion

When true, the check will warn on narrowing integer to floating-point conversion (e.g. size_t to double). true by default.

当为 true 时，此检查项会对整数到浮点数的缩窄转换发出警告（例如 size_t 到 double）。默认值为 true。
:::

::: option
WarnOnFloatingPointNarrowingConversion

When true, the check will warn on narrowing floating point conversion (e.g. double to float). true by default.

当为 true 时，此检查项会对浮点数的缩窄转换发出警告（例如 double 到 float）。默认值为 true。
:::

::: option
WarnWithinTemplateInstantiation

When true, the check will warn on narrowing conversions within template instantiations. false by default.

当为 true 时，此检查项会对模板实例化中的缩窄转换发出警告。默认值为 false。
:::

::: option
WarnOnEquivalentBitWidth

When true, the check will warn on narrowing conversions that arise from casting between types of equivalent bit width. (e.g. int n = uint(0); or long long n = double(0);) true by default.

当为 true 时，此检查项会对等位宽类型之间的缩窄转换发出警告（例如 int n = uint(0); 或 long long n = double(0);）。默认值为 true。
:::

::: option
IgnoreConversionFromTypes

Narrowing conversions from any type in this semicolon-separated list will be ignored. This may be useful to weed out commonly occurring, but less commonly problematic assignments such as int n = std::vector<char>().size(); or int n = std::difference(it1, it2);. The default list is empty, but one suggested list for a legacy codebase would be size_t;ptrdiff_t;size_type;difference_type.

来自此分号分隔列表中任意类型的缩窄转换将被忽略。这对于排除一些常见但问题较小的赋值操作可能很有用，例如 int n = std::vector<char>().size(); 或 int n = std::difference(it1, it2);。默认列表为空，但对于遗留代码库，建议的列表为 size_t;ptrdiff_t;size_type;difference_type。
:::

::: option
PedanticMode

When true, the check will warn on assigning a floating point constant to an integer value even if the floating point value is exactly representable in the destination type (e.g. int i = 1.0;). false by default.

当为 true 时，即使浮点常量在目标类型中可以精确表示，此检查项也会在将其赋值给整数时发出警告（例如 int i = 1.0;）。默认值为 false。
:::

## FAQ

## 常见问题

> What does "narrowing conversion from 'int' to 'float'" mean?

An IEEE754 Floating Point number can represent all integer values in the range [-2^PrecisionBits, 2^PrecisionBits] where PrecisionBits is the number of bits in the mantissa.

For float this would be [-2^23, 2^23], where int can represent values in the range [-2^31, 2^31-1].

> “从 'int' 到 'float' 的缩窄转换” 是什么意思？

IEEE754 浮点数可以表示范围为 [-2^PrecisionBits, 2^PrecisionBits] 的所有整数值，其中 PrecisionBits 是尾数的位数。

对于 float，该范围为 [-2^23, 2^23]，而 int 可以表示的范围是 [-2^31, 2^31-1]。

> What does "implementation-defined" mean?

You may have encountered messages like "narrowing conversion from 'unsigned int' to signed type 'int' is implementation-defined". The C/C++ standard does not mandate two's complement for signed integers, and so the compiler is free to define what the semantics are for converting an unsigned integer to signed integer. Clang's implementation uses the two's complement format.

> “实现定义（implementation-defined）” 是什么意思？

你可能遇到过这样的消息：“从 'unsigned int' 到有符号类型 'int' 的缩窄转换是实现定义的”。C/C++ 标准并不强制要求有符号整数使用二进制补码格式，因此编译器可以自由定义将无符号整数转换为有符号整数时的语义。Clang 的实现使用的是二进制补码格式。
