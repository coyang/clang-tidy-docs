以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔：

# readability-magic-numbers

Detects magic numbers, integer or floating point literals that are embedded in code and not introduced via constants or symbols.

检测魔法数字，即嵌入在代码中但未通过常量或符号引入的整数或浮点字面量。

Many coding guidelines advise replacing the magic values with symbolic constants to improve readability. Here are a few references:

许多编码规范建议使用符号常量替代魔法值以提高可读性。以下是一些参考资料：

> - Rule ES.45: Avoid "magic constants"; use symbolic constants in C++ Core Guidelines  
>   规则 ES.45：避免使用“魔法常量”；在 C++ 核心指南中使用符号常量  
>   https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Res-magic

> - Rule 5.1.1 Use symbolic names instead of literal values in code in High Integrity C++  
>   规则 5.1.1：在高完整性 C++ 中使用符号名称代替代码中的字面值  
>   https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard-expressions

> - Item 17 in "C++ Coding Standards: 101 Rules, Guidelines and Best Practices" by Herb Sutter and Andrei Alexandrescu  
>   《C++ 编码标准：101 条规则、指南和最佳实践》第 17 条，作者 Herb Sutter 和 Andrei Alexandrescu

> - Chapter 17 in "Clean Code - A handbook of agile software craftsmanship." by Robert C. Martin  
>   《代码整洁之道：敏捷软件工艺手册》第 17 章，作者 Robert C. Martin

> - Rule 20701 in "TRAIN REAL TIME DATA PROTOCOL Coding Rules" by Armin-Hagen Weiss, Bombardier  
>   Armin-Hagen Weiss（庞巴迪公司）在《列车实时数据协议编码规则》中提出的规则 20701

> - http://wiki.c2.com/?MagicNumber  
>   http://wiki.c2.com/?MagicNumber

Examples of magic values:

魔法值示例：

```c++
template<typename T, size_t N>
struct CustomType {
   T arr[N];
};

struct OtherType {
   CustomType<int, 30> container;
}
CustomType<int, 30> values;

double circleArea = 3.1415926535 * radius * radius;

double totalCharge = 1.08 * itemPrice;

int getAnswer() {
   return -3; // FILENOTFOUND
}

for (int mm = 1; mm <= 12; ++mm) {
   std::cout << month[mm] << '\n';
}
```

Example with magic values refactored:

重构后的魔法值示例：

```c++
template<typename T, size_t N>
struct CustomType {
   T arr[N];
};

const size_t NUMBER_OF_ELEMENTS = 30;
using containerType = CustomType<int, NUMBER_OF_ELEMENTS>;

struct OtherType {
   containerType container;
}
containerType values;

double circleArea = M_PI * radius * radius;

const double TAX_RATE = 0.08;  // or make it variable and read from a file
                              // 或者将其设为变量并从文件中读取

double totalCharge = (1.0 + TAX_RATE) * itemPrice;

int getAnswer() {
   return E_FILE_NOT_FOUND;
}

for (int mm = 1; mm <= MONTHS_IN_A_YEAR; ++mm) {
   std::cout << month[mm] << '\n';
}
```

For integral literals by default only 0 and 1 (and -1) integer values are accepted without a warning. This can be overridden with the IgnoredIntegerValues option. Negative values are accepted if their absolute value is present in the IgnoredIntegerValues list.

对于整数字面量，默认情况下仅接受 0 和 1（以及 -1）而不会发出警告。可以通过 IgnoredIntegerValues 选项进行覆盖。如果负值的绝对值在 IgnoredIntegerValues 列表中，也会被接受。

As a special case for integral values, all powers of two can be accepted without warning by enabling the IgnorePowersOf2IntegerValues option.

作为整数值的特殊情况，启用 IgnorePowersOf2IntegerValues 选项后，所有 2 的幂将被接受且不会发出警告。

For floating point literals by default the 0.0 floating point value is accepted without a warning. The set of ignored floating point literals can be configured using the IgnoredFloatingPointValues option. For each value in that set, the given string value is converted to a floating-point value representation used by the target architecture. If a floating-point literal value compares equal to one of the converted values, then that literal is not diagnosed by this check. Because floating-point equality is used to determine whether to diagnose or not, the user needs to be aware of the details of floating-point representations for any values that cannot be precisely represented for their target architecture.

对于浮点字面量，默认情况下 0.0 会被接受且不会发出警告。可以使用 IgnoredFloatingPointValues 选项配置被忽略的浮点字面量集合。对于该集合中的每个值，给定的字符串值将被转换为目标架构使用的浮点表示形式。如果某个浮点字面量与转换后的值相等，则不会被诊断。由于使用浮点相等性来判断是否诊断，用户需要了解其目标架构中无法精确表示的浮点值的表示细节。

For each value in the IgnoredFloatingPointValues set, both the single-precision form and double-precision form are accepted (for example, if 3.14 is in the set, neither 3.14f nor 3.14 will produce a warning).

对于 IgnoredFloatingPointValues 集合中的每个值，单精度和双精度形式都将被接受（例如，如果集合中包含 3.14，则 3.14f 和 3.14 都不会产生警告）。

Scientific notation is supported for both source code input and option. Alternatively, the check for the floating point numbers can be disabled for all floating point values by enabling the IgnoreAllFloatingPointValues option.

源代码输入和选项都支持科学计数法。或者，可以通过启用 IgnoreAllFloatingPointValues 选项来禁用对所有浮点值的检查。

Since values 0 and 0.0 are so common as the base counter of loops, or initialization values for sums, they are always accepted without warning, even if not present in the respective ignored values list.

由于 0 和 0.0 通常用作循环的初始计数器或求和的初始值，因此它们始终被接受且不会发出警告，即使它们不在相应的忽略值列表中。

## Options

选项

::: option
IgnoredIntegerValues

Semicolon-separated list of magic positive integers that will be accepted without a warning. Default values are {1, 2, 3, 4}, and 0 is accepted unconditionally.

以分号分隔的魔法正整数列表，这些值将被接受且不会发出警告。默认值为 {1, 2, 3, 4}，0 无条件被接受。
:::

::: option
IgnorePowersOf2IntegerValues

Boolean value indicating whether to accept all powers-of-two integer values without warning. Default value is false.

布尔值，指示是否接受所有 2 的幂的整数值而不发出警告。默认值为 false。
:::

::: option
IgnoredFloatingPointValues

Semicolon-separated list of magic positive floating point values that will be accepted without a warning. Default values are {1.0, 100.0} and 0.0 is accepted unconditionally.

以分号分隔的魔法正浮点值列表，这些值将被接受且不会发出警告。默认值为 {1.0, 100.0}，0.0 无条件被接受。
:::

::: option
IgnoreAllFloatingPointValues

Boolean value indicating whether to accept all floating point values without warning. Default value is false.

布尔值，指示是否接受所有浮点值而不发出警告。默认值为 false。
:::

::: option
IgnoreBitFieldsWidths

Boolean value indicating whether to accept magic numbers as bit field widths without warning. This is useful for example for register definitions which are generated from hardware specifications. Default value is true.

布尔值，指示是否接受作为位字段宽度的魔法数字而不发出警告。这在例如根据硬件规范生成的寄存器定义中非常有用。默认值为 true。
:::

::: option
IgnoreTypeAliases

Boolean value indicating whether to accept magic numbers in typedef or using declarations. Default value is false.

布尔值，指示是否接受 typedef 或 using 声明中的魔法数字。默认值为 false。
:::

::: option
IgnoreUserDefinedLiterals

Boolean value indicating whether to accept magic numbers in user-defined literals. Default value is false.

布尔值，指示是否接受用户自定义字面量中的魔法数字。默认值为 false。
:::
