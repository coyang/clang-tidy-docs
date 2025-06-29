# modernize-use-equals-default

# modernize-use-equals-default

This check replaces default bodies of special member functions with = default;. The explicitly defaulted function declarations enable more opportunities in optimization, because the compiler might treat explicitly defaulted functions as trivial.

此检查会将特殊成员函数的默认函数体替换为 = default;。显式默认的函数声明可以为优化提供更多机会，因为编译器可能会将显式默认的函数视为平凡的（trivial）。

```c++
struct A {
  A() {}
  ~A();
};
A::~A() {}

// becomes

struct A {
  A() = default;
  ~A();
};
A::~A() = default;
```

::: note

Move-constructor and move-assignment operator are not supported yet.

目前尚不支持移动构造函数和移动赋值运算符。

:::

## Options

## 选项

::: option
IgnoreMacros

If set to true, the check will not give warnings inside macros and will ignore special members with bodies contain macros or preprocessor directives. Default is true.

如果设置为 true，该检查将不会在宏内部发出警告，并且会忽略函数体中包含宏或预处理指令的特殊成员函数。默认值为 true。

:::
