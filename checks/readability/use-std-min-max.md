# readability-use-std-min-max

Replaces certain conditional statements with equivalent calls to
`std::min` or `std::max`. Note: This may impact performance in critical
code due to potential additional stores compared to the original if
statement.

Before:

```c++
void foo() {
  int a = 2, b = 3;
  if (a < b)
    a = b;
}
```

After:

```c++
void foo() {
  int a = 2, b = 3;
  a = std::max(a, b);
}
```
