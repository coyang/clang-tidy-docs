# google-runtime-int

Finds uses of short, long and long long and suggest replacing them with u?intXX(\_t)?.

查找使用 short、long 和 long long 的情况，并建议将它们替换为 u?intXX(\_t)?。

The corresponding style guide rule:  
<https://google.github.io/styleguide/cppguide.html#Integer_Types>.

对应的风格指南规则：  
<https://google.github.io/styleguide/cppguide.html#Integer_Types>。

Corresponding cpplint.py check: [runtime/int]{.title-ref}.

对应的 cpplint.py 检查项：[runtime/int]{.title-ref}。

## Options

## 选项

::: option  
UnsignedTypePrefix

A string specifying the unsigned type prefix. Default is [uint]{.title-ref}.

指定无符号类型前缀的字符串。默认值为 [uint]{.title-ref}。  
:::

::: option  
SignedTypePrefix

A string specifying the signed type prefix. Default is [int]{.title-ref}.

指定有符号类型前缀的字符串。默认值为 [int]{.title-ref}。  
:::

::: option  
TypeSuffix

A string specifying the type suffix. Default is an empty string.

指定类型后缀的字符串。默认值为空字符串。  
:::
