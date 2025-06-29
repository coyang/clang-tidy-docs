# bugprone-forward-declaration-namespace

Checks if an unused forward declaration is in a wrong namespace.

检查未使用的前向声明是否位于错误的命名空间中。

The check inspects all unused forward declarations and checks if there is any declaration/definition with the same name existing, which could indicate that the forward declaration is in a potentially wrong namespace.

该检查会分析所有未使用的前向声明，并检查是否存在具有相同名称的声明或定义，这可能表明该前向声明位于一个潜在错误的命名空间中。

```c++
namespace na { struct A; }
namespace nb { struct A {}; }
nb::A a;
// warning : no definition found for 'A', but a definition with the same name
// 'A' found in another namespace 'nb::'
```

```c++
namespace na { struct A; }
namespace nb { struct A {}; }
nb::A a;
// 警告：未找到 'A' 的定义，但在另一个命名空间 'nb::' 中找到了同名定义 'A'
```

This check can only generate warnings, but it can't suggest a fix at this point.

该检查目前只能生成警告，但无法提供修复建议。
