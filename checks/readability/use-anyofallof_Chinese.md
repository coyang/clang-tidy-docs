# readability-use-anyofallof

Finds range-based for loops that can be replaced by a call to
std::any_of or std::all_of. In C++20 mode, suggests
std::ranges::any_of or std::ranges::all_of.

查找可以用 std::any_of 或 std::all_of 替代的基于范围的 for 循环。在 C++20 模式下，建议使用 std::ranges::any_of 或 std::ranges::all_of。

Example:

示例：

```c++
bool all_even(std::vector<int> V) {
  for (int I : V) {
    if (I % 2)
      return false;
  }
  return true;
  // Replace loop by
  // return std::ranges::all_of(V, [](int I) { return I % 2 == 0; });
  // 可替换循环为
  // return std::ranges::all_of(V, [](int I) { return I % 2 == 0; });
}
```
