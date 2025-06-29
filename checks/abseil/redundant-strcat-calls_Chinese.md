# abseil-redundant-strcat-calls

Suggests removal of unnecessary calls to absl::StrCat when the result is being passed to another call to absl::StrCat or absl::StrAppend.

建议移除在将结果传递给另一个 absl::StrCat 或 absl::StrAppend 调用时对 absl::StrCat 的不必要调用。

The extra calls cause unnecessary temporary strings to be constructed. Removing them makes the code smaller and faster.

这些多余的调用会导致构造不必要的临时字符串。移除它们可以使代码更小、更快。

Examples:

示例：

```c++
std::string s = absl::StrCat("A", absl::StrCat("B", absl::StrCat("C", "D")));
//before
//之前

std::string s = absl::StrCat("A", "B", "C", "D");
//after
//之后

absl::StrAppend(&s, absl::StrCat("E", "F", "G"));
//before
//之前

absl::StrAppend(&s, "E", "F", "G");
//after
//之后
```
