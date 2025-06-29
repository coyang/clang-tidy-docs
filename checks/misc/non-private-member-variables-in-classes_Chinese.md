# misc-non-private-member-variables-in-classes

# misc-non-private-member-variables-in-classes

[cppcoreguidelines-non-private-member-variables-in-classes]{.title-ref} redirects here as an alias for this check.

[cppcoreguidelines-non-private-member-variables-in-classes]{.title-ref} 是此检查项的别名，会重定向至此。

Finds classes that contain non-static data members in addition to user-declared non-static member functions and diagnose all data members declared with a non- public access specifier. The data members should be declared as private and accessed through member functions instead of exposed to derived classes or class consumers.

查找包含非静态数据成员以及用户声明的非静态成员函数的类，并诊断所有使用非 public 访问说明符声明的数据成员。数据成员应声明为 private，并通过成员函数访问，而不是暴露给派生类或类的使用者。

## Options

## 选项

::: option
IgnoreClassesWithAllMemberVariablesBeingPublic

当 [true]{.title-ref} 时，如果类中所有成员变量都使用 public 访问说明符声明，则完全忽略该类。默认值为 [false]{.title-ref}。

When [true]{.title-ref}, allows to completely ignore classes if all the member variables in that class declared with a public access specifier. Default is [false]{.title-ref}.
:::

::: option
IgnorePublicMemberVariables

当 [true]{.title-ref} 时，允许忽略（不诊断）所有使用 public 访问说明符声明的成员变量。默认值为 [false]{.title-ref}。

When [true]{.title-ref}, allows to ignore (not diagnose) all the member variables declared with a public access specifier. Default is [false]{.title-ref}.
:::
