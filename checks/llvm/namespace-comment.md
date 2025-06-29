# llvm-namespace-comment

[google-readability-namespace-comments]{.title-ref} redirects here as an
alias for this check.

Checks that long namespaces have a closing comment.

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

## Options

::: option
ShortNamespaceLines

Requires the closing brace of the namespace definition to be followed by
a closing comment if the body of the namespace has more than
[ShortNamespaceLines]{.title-ref} lines of code. The value is an
unsigned integer that defaults to [1U]{.title-ref}.
:::

::: option
SpacesBeforeComments

An unsigned integer specifying the number of spaces before the comment
closing a namespace definition. Default is [1U]{.title-ref}.
:::

::: option
AllowOmittingNamespaceComments

When [true]{.title-ref}, the check will accept if no namespace comment
is present. The check will only fail if the specified namespace comment
is different than expected. Default is [false]{.title-ref}.
:::
