# readability-magic-numbers

Detects magic numbers, integer or floating point literals that are
embedded in code and not introduced via constants or symbols.

Many coding guidelines advise replacing the magic values with symbolic
constants to improve readability. Here are a few references:

> - [Rule ES.45: Avoid \"magic constants\"; use symbolic constants in
>   C++ Core
>   Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Res-magic)
> - [Rule 5.1.1 Use symbolic names instead of literal values in code
>   in High Integrity
>   C++](https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard-expressions)
> - Item 17 in \"C++ Coding Standards: 101 Rules, Guidelines and Best
>   Practices\" by Herb Sutter and Andrei Alexandrescu
> - Chapter 17 in \"Clean Code - A handbook of agile software
>   craftsmanship.\" by Robert C. Martin
> - Rule 20701 in \"TRAIN REAL TIME DATA PROTOCOL Coding Rules\" by
>   Armin-Hagen Weiss, Bombardier
> - <http://wiki.c2.com/?MagicNumber>

Examples of magic values:

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

double totalCharge = (1.0 + TAX_RATE) * itemPrice;

int getAnswer() {
   return E_FILE_NOT_FOUND;
}

for (int mm = 1; mm <= MONTHS_IN_A_YEAR; ++mm) {
   std::cout << month[mm] << '\n';
}
```

For integral literals by default only [0]{.title-ref} and
[1]{.title-ref} (and [-1]{.title-ref}) integer values are accepted
without a warning. This can be overridden with the
`IgnoredIntegerValues`{.interpreted-text role="option"} option. Negative
values are accepted if their absolute value is present in the
`IgnoredIntegerValues`{.interpreted-text role="option"} list.

As a special case for integral values, all powers of two can be accepted
without warning by enabling the
`IgnorePowersOf2IntegerValues`{.interpreted-text role="option"} option.

For floating point literals by default the [0.0]{.title-ref} floating
point value is accepted without a warning. The set of ignored floating
point literals can be configured using the
`IgnoredFloatingPointValues`{.interpreted-text role="option"} option.
For each value in that set, the given string value is converted to a
floating-point value representation used by the target architecture. If
a floating-point literal value compares equal to one of the converted
values, then that literal is not diagnosed by this check. Because
floating-point equality is used to determine whether to diagnose or not,
the user needs to be aware of the details of floating-point
representations for any values that cannot be precisely represented for
their target architecture.

For each value in the `IgnoredFloatingPointValues`{.interpreted-text
role="option"} set, both the single-precision form and double-precision
form are accepted (for example, if 3.14 is in the set, neither 3.14f nor
3.14 will produce a warning).

Scientific notation is supported for both source code input and option.
Alternatively, the check for the floating point numbers can be disabled
for all floating point values by enabling the
`IgnoreAllFloatingPointValues`{.interpreted-text role="option"} option.

Since values [0]{.title-ref} and [0.0]{.title-ref} are so common as the
base counter of loops, or initialization values for sums, they are
always accepted without warning, even if not present in the respective
ignored values list.

## Options

::: option
IgnoredIntegerValues

Semicolon-separated list of magic positive integers that will be
accepted without a warning. Default values are [{1, 2, 3,
4}]{.title-ref}, and [0]{.title-ref} is accepted unconditionally.
:::

::: option
IgnorePowersOf2IntegerValues

Boolean value indicating whether to accept all powers-of-two integer
values without warning. Default value is [false]{.title-ref}.
:::

::: option
IgnoredFloatingPointValues

Semicolon-separated list of magic positive floating point values that
will be accepted without a warning. Default values are [{1.0,
100.0}]{.title-ref} and [0.0]{.title-ref} is accepted unconditionally.
:::

::: option
IgnoreAllFloatingPointValues

Boolean value indicating whether to accept all floating point values
without warning. Default value is [false]{.title-ref}.
:::

::: option
IgnoreBitFieldsWidths

Boolean value indicating whether to accept magic numbers as bit field
widths without warning. This is useful for example for register
definitions which are generated from hardware specifications. Default
value is [true]{.title-ref}.
:::

::: option
IgnoreTypeAliases

Boolean value indicating whether to accept magic numbers in `typedef` or
`using` declarations. Default value is [false]{.title-ref}.
:::

::: option
IgnoreUserDefinedLiterals

Boolean value indicating whether to accept magic numbers in user-defined
literals. Default value is [false]{.title-ref}.
:::
