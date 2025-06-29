# bugprone-narrowing-conversions

[cppcoreguidelines-narrowing-conversions]{.title-ref} redirects here as
an alias for this check.

Checks for silent narrowing conversions, e.g: `int i = 0; i += 0.1;`.
While the issue is obvious in this former example, it might not be so in
the following: `void MyClass::f(double d) { int_member_ += d; }`.

We flag narrowing conversions from:

: - an integer to a narrower integer (e.g. `char` to
`unsigned char`) if WarnOnIntegerNarrowingConversion Option is
set, - an integer to a narrower floating-point (e.g. `uint64_t` to
`float`) if WarnOnIntegerToFloatingPointNarrowingConversion
Option is set, - a floating-point to an integer (e.g. `double` to `int`), - a floating-point to a narrower floating-point (e.g. `double` to
`float`) if WarnOnFloatingPointNarrowingConversion Option is
set.

This check will flag:

: - All narrowing conversions that are not marked by an explicit
cast (c-style or `static_cast`). For example:
`int i = 0; i += 0.1;`, `void f(int); f(0.1);`, - All applications of binary operators with a narrowing
conversions. For example: `int i; i+= 0.1;`.

Arithmetic with smaller integer types than `int` trigger implicit
conversions, as explained under [\"Integral Promotion\" on
cppreference.com](https://en.cppreference.com/w/cpp/language/implicit_conversion).
This check diagnoses more instances of narrowing than the compiler
warning [-Wconversion]{.title-ref} does. The example below demonstrates
this behavior.

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

## Options

::: option
WarnOnIntegerNarrowingConversion

When [true]{.title-ref}, the check will warn on narrowing integer
conversion (e.g. `int` to `size_t`). [true]{.title-ref} by default.
:::

::: option
WarnOnIntegerToFloatingPointNarrowingConversion

When [true]{.title-ref}, the check will warn on narrowing integer to
floating-point conversion (e.g. `size_t` to `double`).
[true]{.title-ref} by default.
:::

::: option
WarnOnFloatingPointNarrowingConversion

When [true]{.title-ref}, the check will warn on narrowing floating point
conversion (e.g. `double` to `float`). [true]{.title-ref} by default.
:::

::: option
WarnWithinTemplateInstantiation

When [true]{.title-ref}, the check will warn on narrowing conversions
within template instantiations. [false]{.title-ref} by default.
:::

::: option
WarnOnEquivalentBitWidth

When [true]{.title-ref}, the check will warn on narrowing conversions
that arise from casting between types of equivalent bit width. (e.g.
[int n = uint(0);]{.title-ref} or [long long n =
double(0);]{.title-ref}) [true]{.title-ref} by default.
:::

::: option
IgnoreConversionFromTypes

Narrowing conversions from any type in this semicolon-separated list
will be ignored. This may be useful to weed out commonly occurring, but
less commonly problematic assignments such as [int n =
std::vector\<char\>().size();]{.title-ref} or [int n =
std::difference(it1, it2);]{.title-ref}. The default list is empty, but
one suggested list for a legacy codebase would be
[size_t;ptrdiff_t;size_type;difference_type]{.title-ref}.
:::

::: option
PedanticMode

When [true]{.title-ref}, the check will warn on assigning a floating
point constant to an integer value even if the floating point value is
exactly representable in the destination type (e.g. `int i = 1.0;`).
[false]{.title-ref} by default.
:::

## FAQ

> - What does \"narrowing conversion from \'int\' to \'float\'\" mean?

An IEEE754 Floating Point number can represent all integer values in the
range \[-2\^PrecisionBits, 2\^PrecisionBits\] where PrecisionBits is the
number of bits in the mantissa.

For `float` this would be \[-2\^23, 2\^23\], where `int` can represent
values in the range \[-2\^31, 2\^31-1\].

> - What does \"implementation-defined\" mean?

You may have encountered messages like \"narrowing conversion from
\'unsigned int\' to signed type \'int\' is implementation-defined\". The
C/C++ standard does not mandate two\'s complement for signed integers,
and so the compiler is free to define what the semantics are for
converting an unsigned integer to signed integer. Clang\'s
implementation uses the two\'s complement format.
