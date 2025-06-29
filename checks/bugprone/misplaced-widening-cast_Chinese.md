# bugprone-misplaced-widening-cast

This check will warn when there is a cast of a calculation result to a bigger type. If the intention of the cast is to avoid loss of precision then the cast is misplaced, and there can be loss of precision. Otherwise the cast is ineffective.

该检查会在将计算结果强制转换为更大类型时发出警告。如果强制转换的目的是为了避免精度丢失，那么该转换的位置是错误的，可能会导致精度丢失。否则，该转换是无效的。

Example code:

示例代码：

```c++
long f(int x) {
    return (long)(x * 1000);
}
```

The result x \* 1000 is first calculated using int precision. If the result exceeds int precision there is loss of precision. Then the result is casted to long.

表达式 x \* 1000 首先使用 int 精度进行计算。如果结果超出了 int 的精度范围，就会发生精度丢失。然后结果被强制转换为 long 类型。

If there is no loss of precision then the cast can be removed or you can explicitly cast to int instead.

如果没有发生精度丢失，那么可以去掉强制转换，或者显式地转换为 int 类型。

If you want to avoid loss of precision then put the cast in a proper location, for instance:

如果你想避免精度丢失，应将强制转换放在正确的位置，例如：

```c++
long f(int x) {
    return (long)x * 1000;
}
```

## Implicit casts

忘记添加强制转换与放错位置一样危险，也同样常见。如果启用了 CheckImplicitCasts 选项，该检查也会检测此类情况，例如：

Forgetting to place the cast at all is at least as dangerous and at least as common as misplacing it. If CheckImplicitCasts is enabled the check also detects these cases, for instance:

```c++
long f(int x) {
    return x * 1000;
}
```

```c++
long f(int x) {
    return x * 1000;
}
```

## Floating point

Currently warnings are only written for integer conversion. No warning is written for this code:

目前该检查仅对整数类型转换发出警告。对于如下代码，不会发出警告：

```c++
double f(float x) {
    return (double)(x * 10.0f);
}
```

```c++
double f(float x) {
    return (double)(x * 10.0f);
}
```

## Options

## 选项

::: option
CheckImplicitCasts

If true, enables detection of implicit casts. Default is false.

如果为 true，则启用对隐式类型转换的检测。默认值为 false。
:::
