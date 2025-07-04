# cppcoreguidelines-pro-bounds-constant-array-index

This check flags all array subscript expressions on static arrays and
`std::arrays` that either do not have a constant integer expression
index or are out of bounds (for `std::array`). For out-of-bounds
checking of static arrays, see the [-Warray-bounds]{.title-ref} Clang
diagnostic.

This rule is part of the [Bounds safety (Bounds 2)](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Pro-bounds-arrayindex)
profile from the C++ Core Guidelines.

Optionally, this check can generate fixes using `gsl::at` for indexing.

## Options

::: option
GslHeader

The check can generate fixes after this option has been set to the name
of the include file that contains `gsl::at()`, e.g.
[\"gsl/gsl.h\"]{.title-ref}. Default is an empty string.
:::

::: option
IncludeStyle

A string specifying which include-style is used, [llvm]{.title-ref} or
[google]{.title-ref}. Default is [llvm]{.title-ref}.
:::
