# cppcoreguidelines-avoid-non-const-global-variables

Finds non-const global variables as described in I.2 of C++ Core Guidelines. As R.6 of C++ Core Guidelines is a duplicate of rule I.2 it also covers that rule.

查找 C++ 核心指南中 I.2 所描述的非常量全局变量。由于核心指南中的 R.6 与规则 I.2 重复，因此本规则也涵盖 R.6。

```c++
char a;  // Warns!
const char b =  0;

namespace some_namespace
{
    char c;  // Warns!
    const char d = 0;
}

char * c_ptr1 = &some_namespace::c;  // Warns!
char *const c_const_ptr = &some_namespace::c;  // Warns!
char & c_reference = some_namespace::c;  // Warns!

class Foo  // No Warnings inside Foo, only namespace scope is covered
{
public:
    char e = 0;
    const char f = 0;
protected:
    char g = 0;
private:
    char h = 0;
};
```

The variables a, c, c_ptr1, c_const_ptr and c_reference will all generate warnings since they are either a non-const globally accessible variable, a pointer or a reference providing global access to non-const data or both.

变量 a、c、c_ptr1、c_const_ptr 和 c_reference 都会触发警告，因为它们要么是非常量的全局可访问变量，要么是指向非常量数据的指针或引用，从而提供了对非常量数据的全局访问，或者两者兼有。

## Options

## 选项

::: option
AllowInternalLinkage

When set to true, static non-const variables and variables in anonymous namespaces will not generate a warning. The default value is false.

当设置为 true 时，静态非常量变量以及匿名命名空间中的变量将不会触发警告。默认值为 false。
:::
