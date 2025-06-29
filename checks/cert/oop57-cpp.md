# cert-oop57-cpp

> Flags use of the [C]{.title-ref} standard library functions `memset`,
> `memcpy` and `memcmp` and similar derivatives on non-trivial types.

## Options

::: option
MemSetNames

Specify extra functions to flag that act similarly to `memset`. Specify
names in a semicolon delimited list. Default is an empty string. The
check will detect the following functions: [memset]{.title-ref},
[std::memset]{.title-ref}.
:::

::: option
MemCpyNames

Specify extra functions to flag that act similarly to `memcpy`. Specify
names in a semicolon delimited list. Default is an empty string. The
check will detect the following functions: [std::memcpy]{.title-ref},
[memcpy]{.title-ref}, [std::memmove]{.title-ref}, [memmove]{.title-ref},
[std::strcpy]{.title-ref}, [strcpy]{.title-ref}, [memccpy]{.title-ref},
[stpncpy]{.title-ref}, [strncpy]{.title-ref}.
:::

::: option
MemCmpNames

Specify extra functions to flag that act similarly to `memcmp`. Specify
names in a semicolon delimited list. Default is an empty string. The
check will detect the following functions: [std::memcmp]{.title-ref},
[memcmp]{.title-ref}, [std::strcmp]{.title-ref}, [strcmp]{.title-ref},
[strncmp]{.title-ref}.
:::

This check corresponds to the CERT C++ Coding Standard rule [OOP57-CPP.
Prefer special member functions and overloaded operators to C Standard
Library
functions](https://wiki.sei.cmu.edu/confluence/display/cplusplus/OOP57-CPP.+Prefer+special+member+functions+and+overloaded+operators+to+C+Standard+Library+functions).
