# readability-redundant-casting

Detects explicit type casting operations that involve the same source and destination types, and subsequently recommend their removal. Covers a range of explicit casting operations, including static_cast, const_cast, C-style casts, and reinterpret_cast. Its primary objective is to enhance code readability and maintainability by eliminating unnecessary type casting.

检测源类型和目标类型相同的显式类型转换操作，并建议将其移除。涵盖多种显式类型转换操作，包括 static_cast、const_cast、C 风格转换以及 reinterpret_cast。其主要目标是通过消除不必要的类型转换来提高代码的可读性和可维护性。

```c++
int value = 42;
int result = static_cast<int>(value);
```

In this example, the static_cast<int>(value) is redundant, as it performs a cast from an int to another int.

在此示例中，static_cast<int>(value) 是冗余的，因为它将一个 int 类型转换为另一个 int 类型。

Casting operations involving constructor conversions, user-defined conversions, functional casts, type-dependent casts, casts between distinct type aliases that refer to the same underlying type, as well as bitfield-related casts and casts directly from lvalue to rvalue, are all disregarded by the check.

此检查不会考虑以下类型转换操作：构造函数转换、用户自定义转换、函数式转换、依赖类型的转换、指向相同底层类型的不同类型别名之间的转换、位域相关的转换以及从左值到右值的直接转换。

## Options

## 选项

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，则该检查不会在宏内部发出警告。默认值为 true。
:::

::: option
IgnoreTypeAliases

When set to false, the check will consider type aliases, and when set to true, it will resolve all type aliases and operate on the underlying types. Default is false.

当设置为 false 时，该检查将考虑类型别名；当设置为 true 时，它将解析所有类型别名并基于底层类型进行操作。默认值为 false。
:::
