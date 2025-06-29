# readability-const-return-type

Checks for functions with a const-qualified return type and recommends removal of the const keyword. Such use of const is usually superfluous, and can prevent valuable compiler optimizations. Does not (yet) fix trailing return types.

检查具有 const 限定返回类型的函数，并建议移除 const 关键字。这种对 const 的使用通常是多余的，并且可能会阻止有价值的编译器优化。目前尚不支持修复尾置返回类型。

Examples:

示例：

```c++
const int foo();
const Clazz foo();
Clazz *const foo();
```

Note that this applies strictly to top-level qualification, which excludes pointers or references to const values. For example, these are fine:

请注意，这仅适用于顶层限定，不包括指向 const 值的指针或引用。例如，以下是可以接受的：

```c++
const int* foo();
const int& foo();
const Clazz* foo();
```

## Options

## 选项

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros. Default is true.

如果设置为 true，则该检查不会在宏内部发出警告。默认值为 true。
:::
