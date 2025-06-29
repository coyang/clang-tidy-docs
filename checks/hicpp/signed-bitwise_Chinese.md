# hicpp-signed-bitwise

Finds uses of bitwise operations on signed integer types, which may lead  
to undefined or implementation defined behavior.

查找对有符号整数类型进行按位操作的用法，这可能导致未定义或实现定义的行为。

The according rule is defined in the [High Integrity C++ Standard,  
Section  
5.6.1](https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard-expressions).

相关规则定义在 [高完整性 C++ 标准，第 5.6.1 节](https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard-expressions)。

## Options

## 选项

::: option  
IgnorePositiveIntegerLiterals

If this option is set to true, the check will not warn on  
bitwise operations with positive integer literals, e.g.  
~0, 2 << 1, etc. Default value is false.

如果此选项设置为 true，则不会对使用正整数字面量的按位操作发出警告，例如 ~0、2 << 1 等。默认值为 false。  
:::
