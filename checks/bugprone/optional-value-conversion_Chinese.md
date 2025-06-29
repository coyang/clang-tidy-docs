# bugprone-optional-value-conversion

Detects potentially unintentional and redundant conversions where a value is extracted from an optional-like type and then used to create a new instance of the same optional-like type.

检测潜在的无意且冗余的转换，即从一个 optional 类类型中提取值后，又用该值创建相同类型的新实例。

These conversions might be the result of developer oversight, leftovers from code refactoring, or other situations that could lead to unintended exceptions or cases where the resulting optional is always initialized, which might be unexpected behavior.

这些转换可能是由于开发者疏忽、重构代码时遗留的问题，或其他情况导致的。这可能会引发意外的异常，或者导致生成的 optional 始终被初始化，从而产生非预期的行为。

To illustrate, consider the following problematic code snippet:

例如，考虑以下有问题的代码片段：

```c++
#include <optional>

void print(std::optional<int>);

int main()
{
  std::optional<int> opt;
  // ...

  // Unintentional conversion from std::optional<int> to int and back to
  // std::optional<int>:
  print(opt.value());

  // ...
}
```

A better approach would be to directly pass opt to the print function without extracting its value:

更好的做法是直接将 opt 传递给 print 函数，而不提取其值：

```c++
#include <optional>

void print(std::optional<int>);

int main()
{
  std::optional<int> opt;
  // ...

  // Proposed code: Directly pass the std::optional<int> to the print
  // function.
  print(opt);

  // ...
}
```

By passing opt directly to the print function, unnecessary conversions are avoided, and potential unintended behavior or exceptions are minimized.

通过将 opt 直接传递给 print 函数，可以避免不必要的转换，从而最大限度地减少潜在的非预期行为或异常。

Value extraction using operator \* is matched by default. The support for non-standard optional types such as boost::optional or absl::optional may be limited.

默认情况下会匹配使用 operator \* 进行的值提取。对非标准 optional 类型（如 boost::optional 或 absl::optional）的支持可能有限。

## Options:

## 选项：

::: option
OptionalTypes

Semicolon-separated list of (fully qualified) optional type names or regular expressions that match the optional types. Default value is [::std::optional;::absl::optional;::boost::optional]{.title-ref}.

以分号分隔的 optional 类型名称（需使用完全限定名）或匹配 optional 类型的正则表达式列表。默认值为 [::std::optional;::absl::optional;::boost::optional]{.title-ref}。
:::

::: option
ValueMethods

Semicolon-separated list of (fully qualified) method names or regular expressions that match the methods. Default value is [::value$;::get$]{.title-ref}.

以分号分隔的方法名称（需使用完全限定名）或匹配方法的正则表达式列表。默认值为 [::value$;::get$]{.title-ref}。
:::
