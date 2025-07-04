# hicpp-signed-bitwise

Finds uses of bitwise operations on signed integer types, which may lead
to undefined or implementation defined behavior.

The according rule is defined in the [High Integrity C++ Standard,
Section
5.6.1](https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard-expressions).

## Options

::: option
IgnorePositiveIntegerLiterals

If this option is set to [true]{.title-ref}, the check will not warn on
bitwise operations with positive integer literals, e.g.
[\~0]{.title-ref}, [2 \<\< 1]{.title-ref}, etc. Default value is
[false]{.title-ref}.
:::
