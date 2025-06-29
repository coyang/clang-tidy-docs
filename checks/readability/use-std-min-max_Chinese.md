# readability-use-std-min-max

Replaces certain conditional statements with equivalent calls to  
std::min or std::max. Note: This may impact performance in critical  
code due to potential additional stores compared to the original if  
statement.

替换某些条件语句为等效的 std::min 或 std::max 调用。注意：与原始 if 语句相比，这可能会由于潜在的额外存储操作而影响关键代码的性能。

Before:  
之前：

```c++
void foo() {
  int a = 2, b = 3;
  if (a < b)
    a = b;
}
```

After:  
之后：

```c++
void foo() {
  int a = 2, b = 3;
  a = std::max(a, b);
}
```
