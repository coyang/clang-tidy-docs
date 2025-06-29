# bugprone-bool-pointer-implicit-conversion

Checks for conditions based on implicit conversion from a bool pointer to bool.

检查基于从 bool 指针到 bool 的隐式转换的条件。

Example:

示例：

```c++
bool *p;
if (p) {
  // Never used in a pointer-specific way.
  // 从未以指针特有的方式使用。
}
```
