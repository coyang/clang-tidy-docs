# cppcoreguidelines-use-enum-class

Finds unscoped (non-class) `enum` declarations and suggests using
`enum class` instead.

This check implements
[Enum.3](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Renum-class)
from the C++ Core Guidelines.\"

Example:

```c++
enum E {};        // use "enum class E {};" instead
enum class E {};  // OK

struct S {
    enum E {};    // use "enum class E {};" instead
                  // OK with option IgnoreUnscopedEnumsInClasses
};

namespace N {
    enum E {};    // use "enum class E {};" instead
}
```

## Options

::: option
IgnoreUnscopedEnumsInClasses

When [true]{.title-ref}, ignores unscoped `enum` declarations in
classes. Default is [false]{.title-ref}.
:::
