# cppcoreguidelines-use-enum-class

Finds unscoped (non-class) enum declarations and suggests using enum class instead.

查找未限定作用域（非 class 类型）的 enum 声明，并建议使用 enum class 代替。

This check implements Enum.3 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的规则 Enum.3。

Example:

示例：

```c++
enum E {};        // use "enum class E {};" instead
                  // 建议使用 "enum class E {};" 代替

enum class E {};  // OK
                  // 正确

struct S {
    enum E {};    // use "enum class E {};" instead
                  // 建议使用 "enum class E {};" 代替
                  // OK with option IgnoreUnscopedEnumsInClasses
                  // 如果启用选项 IgnoreUnscopedEnumsInClasses，则此写法也可接受
};

namespace N {
    enum E {};    // use "enum class E {};" instead
                  // 建议使用 "enum class E {};" 代替
}
```

## Options

## 选项

::: option
IgnoreUnscopedEnumsInClasses

When true, ignores unscoped enum declarations in classes. Default is false.

当设置为 true 时，忽略类中的未限定作用域 enum 声明。默认值为 false。
:::
