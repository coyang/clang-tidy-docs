# bugprone-reserved-identifier

# bugprone-reserved-identifier（易出错的保留标识符）

[cert-dcl37-c]{.title-ref} 和 [cert-dcl51-cpp]{.title-ref} 被重定向到此检查项，作为该检查的别名。

[cert-dcl37-c]{.title-ref} 和 [cert-dcl51-cpp]{.title-ref} 被重定向到此检查项，作为该检查的别名。

Checks for usages of identifiers reserved for use by the implementation.

检查使用了被实现保留的标识符的情况。

The C and C++ standards both reserve the following names for such use:

C 和 C++ 标准都为实现保留了以下类型的名称：

- identifiers that begin with an underscore followed by an uppercase letter;
- 以下划线开头，后跟大写字母的标识符；
- identifiers in the global namespace that begin with an underscore.
- 在全局命名空间中以下划线开头的标识符。

The C standard additionally reserves names beginning with a double underscore, while the C++ standard strengthens this to reserve names with a double underscore occurring anywhere.

C 标准还保留了以双下划线开头的名称，而 C++ 标准更进一步，保留了在任何位置包含双下划线的名称。

Violating the naming rules above results in undefined behavior.

违反上述命名规则将导致未定义行为。

```c++
namespace NS {
  void __f(); // name is not allowed in user code
  using _Int = int; // same with this
  #define cool__macro // also this
}
int _g(); // disallowed in global namespace only
```

```c++
namespace NS {
  void __f(); // 用户代码中不允许使用该名称
  using _Int = int; // 同样不允许
  #define cool__macro // 这个也不允许
}
int _g(); // 仅在全局命名空间中不允许
```

The check can also be inverted, i.e. it can be configured to flag any identifier that is not a reserved identifier. This mode is for use by e.g. standard library implementors, to ensure they don't infringe on the user namespace.

该检查也可以反转配置，即可以设置为标记所有不是保留标识符的名称。此模式适用于例如标准库实现者，以确保他们不会侵犯用户命名空间。

This check does not (yet) check for other reserved names, e.g. macro names identical to language keywords, and names specifically reserved by language standards, e.g. C++ 'zombie names' and C future library directions.

该检查目前不会检查其他保留名称，例如与语言关键字相同的宏名称，以及语言标准特别保留的名称，例如 C++ 的“僵尸名称”和 C 的未来库方向。

This check corresponds to CERT C Coding Standard rule DCL37-C. Do not declare or define a reserved identifier as well as its C++ counterpart, DCL51-CPP. Do not declare or define a reserved identifier.

该检查对应于 CERT C 编码标准规则 DCL37-C：不要声明或定义保留标识符，以及其 C++ 对应规则 DCL51-CPP：不要声明或定义保留标识符。

链接：

- https://wiki.sei.cmu.edu/confluence/display/c/DCL37-C.+Do+not+declare+or+define+a+reserved+identifier
- https://wiki.sei.cmu.edu/confluence/display/cplusplus/DCL51-CPP.+Do+not+declare+or+define+a+reserved+identifier

## Options

## 选项

::: option
Invert

Invert

If true, inverts the check, i.e. flags names that are not reserved. Default is false.

如果为 true，则反转检查，即标记所有不是保留名称的标识符。默认值为 false。

:::

::: option
AllowedIdentifiers

AllowedIdentifiers（允许的标识符）

Semicolon-separated list of regular expressions that the check ignores. Default is an empty list.

以分号分隔的正则表达式列表，检查时将忽略匹配的标识符。默认值为空列表。

:::
