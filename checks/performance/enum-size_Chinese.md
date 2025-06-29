# performance-enum-size

Recommends the smallest possible underlying type for an enum or enum class based on the range of its enumerators. Analyzes the values of the enumerators in an enum or enum class, including signed values, to recommend the smallest possible underlying type that can represent all the values of the enum. The suggested underlying types are the integral types std::uint8_t, std::uint16_t, and std::uint32_t for unsigned types, and std::int8_t, std::int16_t, and std::int32_t for signed types. Using the suggested underlying types can help reduce the memory footprint of the program and improve performance in some cases.

根据枚举成员的取值范围，建议为 enum 或 enum class 使用最小可能的底层类型。分析 enum 或 enum class 中枚举成员的值（包括有符号值），以推荐能够表示所有枚举值的最小底层类型。建议的底层类型包括用于无符号类型的整数类型 std::uint8_t、std::uint16_t 和 std::uint32_t，以及用于有符号类型的 std::int8_t、std::int16_t 和 std::int32_t。使用建议的底层类型可以在某些情况下减少程序的内存占用并提升性能。

For example:

例如：

```c++
// BEFORE
enum Color {
    RED = -1,
    GREEN = 0,
    BLUE = 1
};

std::optional<Color> color_opt;
```

The Color enum uses the default underlying type, which is int in this case, and its enumerators have values of -1, 0, and 1. Additionally, the std::optional<Color> object uses 8 bytes due to padding (platform dependent).

Color 枚举使用默认的底层类型，在本例中为 int，其枚举成员的值为 -1、0 和 1。此外，由于填充（取决于平台），std::optional<Color> 对象占用了 8 个字节。

```c++
// AFTER
enum Color : std::int8_t {
    RED = -1,
    GREEN = 0,
    BLUE = 1
}

std::optional<Color> color_opt;
```

In the revised version of the Color enum, the underlying type has been changed to std::int8_t. The enumerator RED has a value of -1, which can be represented by a signed 8-bit integer.

在修改后的 Color 枚举中，底层类型被更改为 std::int8_t。枚举成员 RED 的值为 -1，可以用一个有符号的 8 位整数表示。

By using a smaller underlying type, the memory footprint of the Color enum is reduced from 4 bytes to 1 byte. The revised version of the std::optional<Color> object would only require 2 bytes (due to lack of padding), since it contains a single byte for the Color enum and a single byte for the bool flag that indicates whether the optional value is set.

通过使用更小的底层类型，Color 枚举的内存占用从 4 字节减少到 1 字节。修改后的 std::optional<Color> 对象仅需 2 字节（由于没有填充），因为它包含一个字节用于 Color 枚举，另一个字节用于表示可选值是否被设置的 bool 标志。

Reducing the memory footprint of an enum can have significant benefits in terms of memory usage and cache performance. However, it's important to consider the trade-offs and potential impact on code readability and maintainability.

减少枚举的内存占用在内存使用和缓存性能方面可能带来显著好处。然而，也需要权衡其对代码可读性和可维护性的潜在影响。

Enums without enumerators (empty) are excluded from analysis.

不包含任何枚举成员的枚举类型（空枚举）将被排除在分析之外。

Requires C++11 or above. Does not provide auto-fixes.

需要 C++11 或更高版本。不提供自动修复。

## Options

## 选项

::: option
EnumIgnoreList

Option is used to ignore certain enum types. It accepts a semicolon-separated list of (fully qualified) enum type names or regular expressions that match the enum type names. The default value is an empty string, which means no enums will be ignored.

该选项用于忽略某些枚举类型。它接受一个由分号分隔的（完全限定的）枚举类型名称列表，或匹配枚举类型名称的正则表达式。默认值为空字符串，表示不会忽略任何枚举类型。
:::
