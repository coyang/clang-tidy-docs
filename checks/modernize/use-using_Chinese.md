# modernize-use-using

# modernize-use-using

The check converts the usage of typedef with using keyword.

该检查会将 typedef 的用法转换为 using 关键字。

Before:

之前：

```c++
typedef int variable;

class Class{};
typedef void (Class::* MyPtrType)() const;

typedef struct { int a; } R_t, *R_p;
```

After:

之后：

```c++
using variable = int;

class Class{};
using MyPtrType = void (Class::*)() const;

using R_t = struct { int a; };
using R_p = R_t*;
```

The checker ignores typedef within extern "C" { ... } blocks.

该检查器会忽略 extern "C" { ... } 块中的 typedef。

```c++
extern "C" {
  typedef int InExternC; // Left intact.
}
```

This check requires using C++11 or higher to run.

此检查要求使用 C++11 或更高版本才能运行。

## Options

## 选项

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，该检查不会在宏内部发出警告。默认值为 true。
:::

::: option
IgnoreExternC

If set to true, the check will not give warning inside extern "C" scope. Default is false.

如果设置为 true，该检查不会在 extern "C" 范围内发出警告。默认值为 false。
:::
