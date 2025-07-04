# bugprone-too-small-loop-variable

Detects those `for` loops that have a loop variable with a \"too small\"
type which means this type can\'t represent all values which are part of
the iteration range.

```c++
int main() {
  long size = 294967296l;
  for (short i = 0; i < size; ++i) {}
}
```

This `for` loop is an infinite loop because the `short` type can\'t
represent all values in the `[0..size]` interval.

In a real use case size means a container\'s size which depends on the
user input.

```c++
int doSomething(const std::vector& items) {
  for (short i = 0; i < items.size(); ++i) {}
}
```

This algorithm works for a small amount of objects, but will lead to
freeze for a larger user input.

It\'s recommended to enable the compiler warning
[-Wtautological-constant-out-of-range-compare]{.title-ref} as well,
since check does not inspect compile-time constant loop boundaries to
avoid overlaps with the warning.

## Options

::: option
MagnitudeBitsUpperLimit

Upper limit for the magnitude bits of the loop variable. If it\'s set
the check filters out those catches in which the loop variable\'s type
has more magnitude bits as the specified upper limit. The default value
is 16. For example, if the user sets this option to 31 (bits), then a
32-bit `unsigned int` is ignored by the check, however a 32-bit `int` is
not (A 32-bit `signed int` has 31 magnitude bits).
:::

```c++
int main() {
  long size = 294967296l;
  for (unsigned i = 0; i < size; ++i) {} // no warning with MagnitudeBitsUpperLimit = 31 on a system where unsigned is 32-bit
  for (int i = 0; i < size; ++i) {} // warning with MagnitudeBitsUpperLimit = 31 on a system where int is 32-bit
}
```
