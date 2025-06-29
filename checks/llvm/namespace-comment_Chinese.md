# llvm-namespace-comment

[google-readability-namespace-comments]{.title-ref} redirects here as an alias for this check.

[google-readability-namespace-comments]{.title-ref} 是此检查项的别名，重定向至此。

Checks that long namespaces have a closing comment.

检查较长的命名空间是否具有结束注释。

<https://llvm.org/docs/CodingStandards.html#namespace-indentation>

<https://google.github.io/styleguide/cppguide.html#Namespaces>

```c++
namespace n1 {
void f();
}

// becomes

namespace n1 {
void f();
}  // namespace n1
```

```c++
namespace n1 {
void f();
}

// 转换为

namespace n1 {
void f();
}  // namespace n1
```

## Options

## 选项

::: option
ShortNamespaceLines

Requires the closing brace of the namespace definition to be followed by a closing comment if the body of the namespace has more than [ShortNamespaceLines]{.title-ref} lines of code. The value is an unsigned integer that defaults to [1U]{.title-ref}.
:::

::: option
ShortNamespaceLines

如果命名空间体中的代码行数超过 [ShortNamespaceLines]{.title-ref}，则要求命名空间定义的右大括号后必须跟有结束注释。该值是一个无符号整数，默认值为 [1U]{.title-ref}。
:::

::: option
SpacesBeforeComments

An unsigned integer specifying the number of spaces before the comment closing a namespace definition. Default is [1U]{.title-ref}.
:::

::: option
SpacesBeforeComments

一个无符号整数，用于指定命名空间定义结束注释前的空格数。默认值为 [1U]{.title-ref}。
:::

::: option
AllowOmittingNamespaceComments

When [true]{.title-ref}, the check will accept if no namespace comment is present. The check will only fail if the specified namespace comment is different than expected. Default is [false]{.title-ref}.
:::

::: option
AllowOmittingNamespaceComments

当设置为 [true]{.title-ref} 时，如果没有命名空间注释，检查也会通过。只有在存在命名空间注释但与预期不符时，检查才会失败。默认值为 [false]{.title-ref}。
:::
