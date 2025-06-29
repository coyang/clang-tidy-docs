以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔：

# modernize-use-override

Adds override (introduced in C++11) to overridden virtual functions and removes virtual from those functions as it is not required.

为被重写的虚函数添加 override（在 C++11 中引入），并移除这些函数中的 virtual 关键字，因为它已不再是必需的。

virtual on non base class implementations was used to help indicate to the user that a function was virtual. C++ compilers did not use the presence of this to signify an overridden function.

在非基类的实现中使用 virtual 是为了帮助用户识别该函数为虚函数。C++ 编译器并不会通过 virtual 的存在来判断函数是否为重写。

In C++11 override and final keywords were introduced to allow overridden functions to be marked appropriately. Their presence allows compilers to verify that an overridden function correctly overrides a base class implementation.

在 C++11 中，引入了 override 和 final 关键字，用于对被重写的函数进行适当标记。它们的存在使编译器能够验证被重写的函数是否正确地重写了基类的实现。

This can be useful as compilers can generate a compile time error when:

这非常有用，因为编译器可以在以下情况下生成编译时错误：

> - The base class implementation function signature changes.
> - 基类实现的函数签名发生变化。
> - The user has not created the override with the correct signature.
> - 用户未使用正确的函数签名创建重写函数。

## Options

## 选项

::: option
IgnoreDestructors

If set to true, this check will not diagnose destructors.  
Default is false.

如果设置为 true，此检查将不会诊断析构函数。  
默认值为 false。
:::

::: option
IgnoreTemplateInstantiations

If set to true, instructs this check to ignore virtual function overrides that are part of template instantiations.  
Default is false.

如果设置为 true，此检查将忽略属于模板实例化部分的虚函数重写。  
默认值为 false。
:::

::: option
AllowOverrideAndFinal

If set to true, this check will not diagnose override as redundant with final. This is useful when code will be compiled by a compiler with warning/error checking flags requiring override explicitly on overridden members, such as gcc -Wsuggest-override/gcc -Werror=suggest-override.  
Default is false.

如果设置为 true，此检查将不会将 override 与 final 同时使用视为冗余。这在代码需要使用带有警告/错误检查标志的编译器进行编译时非常有用，例如 gcc -Wsuggest-override 或 gcc -Werror=suggest-override。  
默认值为 false。
:::

::: option
OverrideSpelling

Specifies a macro to use instead of override. This is useful when maintaining source code that also needs to compile with a pre-C++11 compiler.

指定一个宏来替代 override。当需要维护同时兼容 C++11 之前编译器的源代码时，这非常有用。
:::

::: option
FinalSpelling

Specifies a macro to use instead of final. This is useful when maintaining source code that also needs to compile with a pre-C++11 compiler.

指定一个宏来替代 final。当需要维护同时兼容 C++11 之前编译器的源代码时，这非常有用。
:::

::: note

For more information on the use of override see  
https://en.cppreference.com/w/cpp/language/override

关于 override 的更多信息，请参见：  
https://en.cppreference.com/w/cpp/language/override
:::
