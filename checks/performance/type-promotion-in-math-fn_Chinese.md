# performance-type-promotion-in-math-fn

Finds calls to C math library functions (from math.h or, in C++, cmath) with implicit float to double promotions.

查找对 C 数学库函数（来自 math.h，或在 C++ 中来自 cmath）的调用，这些调用中存在隐式的 float 到 double 类型提升。

For example, warns on ::sin(0.f), because this function's parameter is a double. You probably meant to call std::sin(0.f) (in C++), or sinf(0.f) (in C).

例如，会对 ::sin(0.f) 发出警告，因为该函数的参数是 double 类型。你可能本意是调用 std::sin(0.f)（在 C++ 中）或 sinf(0.f)（在 C 中）。

```c++
float a;
asin(a);

// becomes

float a;
std::asin(a);
```

```c++
float a;
asin(a);

// 变为

float a;
std::asin(a);
```
