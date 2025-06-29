# readability-named-parameter

Find functions with unnamed arguments.

查找具有未命名参数的函数。

The check implements the following rule originating in the Google C++ Style Guide:

该检查实现了源自 Google C++ 风格指南中的以下规则：

<https://google.github.io/styleguide/cppguide.html#Function_Declarations_and_Definitions>

All parameters should have the same name in both the function declaration and definition. If a parameter is not utilized, its name can be commented out in a function definition.

所有参数在函数声明和定义中应具有相同的名称。如果某个参数未被使用，可以在函数定义中将其名称注释掉。

```c++
int doingSomething(int a, int b, int c);

int doingSomething(int a, int b, int /*c*/) {
    // Ok: the third param is not used
    // 可以：第三个参数未被使用
    return a + b;
}
```

Corresponding cpplint.py check name: [readability/function]{.title-ref}.

对应的 cpplint.py 检查名称为：[readability/function]{.title-ref}。
