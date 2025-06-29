# cert-oop57-cpp

> Flags use of the [C]{.title-ref} standard library functions memset, memcpy and memcmp and similar derivatives on non-trivial types.

> 标记在非平凡类型上使用 C 标准库函数 memset、memcpy 和 memcmp 及其类似派生函数的情况。

## Options

## 选项

::: option
MemSetNames

Specify extra functions to flag that act similarly to memset. Specify names in a semicolon delimited list. Default is an empty string. The check will detect the following functions: memset, std::memset.

指定额外的函数名称，这些函数的行为类似于 memset。名称应以分号分隔的列表形式提供。默认值为空字符串。该检查将检测以下函数：memset、std::memset。

:::

::: option
MemCpyNames

Specify extra functions to flag that act similarly to memcpy. Specify names in a semicolon delimited list. Default is an empty string. The check will detect the following functions: std::memcpy, memcpy, std::memmove, memmove, std::strcpy, strcpy, memccpy, stpncpy, strncpy.

指定额外的函数名称，这些函数的行为类似于 memcpy。名称应以分号分隔的列表形式提供。默认值为空字符串。该检查将检测以下函数：std::memcpy、memcpy、std::memmove、memmove、std::strcpy、strcpy、memccpy、stpncpy、strncpy。

:::

::: option
MemCmpNames

Specify extra functions to flag that act similarly to memcmp. Specify names in a semicolon delimited list. Default is an empty string. The check will detect the following functions: std::memcmp, memcmp, std::strcmp, strcmp, strncmp.

指定额外的函数名称，这些函数的行为类似于 memcmp。名称应以分号分隔的列表形式提供。默认值为空字符串。该检查将检测以下函数：std::memcmp、memcmp、std::strcmp、strcmp、strncmp。

:::

This check corresponds to the CERT C++ Coding Standard rule OOP57-CPP. Prefer special member functions and overloaded operators to C Standard Library functions.

该检查对应于 CERT C++ 编码标准规则 OOP57-CPP：优先使用特殊成员函数和重载运算符，而不是 C 标准库函数。

链接：https://wiki.sei.cmu.edu/confluence/display/cplusplus/OOP57-CPP.+Prefer+special+member+functions+and+overloaded+operators+to+C+Standard+Library+functions
