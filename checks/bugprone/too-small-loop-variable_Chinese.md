# bugprone-too-small-loop-variable

Detects those for loops that have a loop variable with a "too small" type which means this type can't represent all values which are part of the iteration range.

检测那些 for 循环中使用了“类型过小”的循环变量的情况，这种类型无法表示迭代范围内的所有值。

```c++
int main() {
  long size = 294967296l;
  for (short i = 0; i < size; ++i) {}
}
```

This for loop is an infinite loop because the short type can't represent all values in the [0..size] interval.

这个 for 循环是一个无限循环，因为 short 类型无法表示 [0..size] 区间内的所有值。

In a real use case size means a container's size which depends on the user input.

在实际使用中，size 通常表示容器的大小，该值取决于用户输入。

```c++
int doSomething(const std::vector& items) {
  for (short i = 0; i < items.size(); ++i) {}
}
```

This algorithm works for a small amount of objects, but will lead to freeze for a larger user input.

该算法在处理少量对象时可以正常工作，但在用户输入较大时会导致程序卡死。

It's recommended to enable the compiler warning -Wtautological-constant-out-of-range-compare as well, since check does not inspect compile-time constant loop boundaries to avoid overlaps with the warning.

建议同时启用编译器警告 -Wtautological-constant-out-of-range-compare，因为该检查不会分析编译时常量的循环边界，以避免与该警告重复。

## Options

## 选项

::: option
MagnitudeBitsUpperLimit

Upper limit for the magnitude bits of the loop variable. If it's set the check filters out those catches in which the loop variable's type has more magnitude bits as the specified upper limit. The default value is 16. For example, if the user sets this option to 31 (bits), then a 32-bit unsigned int is ignored by the check, however a 32-bit int is not (A 32-bit signed int has 31 magnitude bits).

循环变量的有效位数上限。如果设置了该选项，检查器会忽略那些循环变量类型的有效位数超过该上限的情况。默认值为 16。例如，如果用户将该选项设置为 31（位），那么一个 32 位的 unsigned int 将不会触发警告，但一个 32 位的 int 会触发（因为 32 位的有符号 int 有 31 个有效位）。
:::

```c++
int main() {
  long size = 294967296l;
  for (unsigned i = 0; i < size; ++i) {} // no warning with MagnitudeBitsUpperLimit = 31 on a system where unsigned is 32-bit
  for (int i = 0; i < size; ++i) {} // warning with MagnitudeBitsUpperLimit = 31 on a system where int is 32-bit
}
```

```c++
int main() {
  long size = 294967296l;
  for (unsigned i = 0; i < size; ++i) {} // 在 unsigned 为 32 位的系统上，设置 MagnitudeBitsUpperLimit = 31 时不会触发警告
  for (int i = 0; i < size; ++i) {} // 在 int 为 32 位的系统上，设置 MagnitudeBitsUpperLimit = 31 时会触发警告
}
```
