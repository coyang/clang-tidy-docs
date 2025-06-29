# abseil-str-cat-append

Flags uses of absl::StrCat() to append to a std::string. Suggests absl::StrAppend() should be used instead.

标记使用 absl::StrCat() 来追加到 std::string 的情况。建议改用 absl::StrAppend()。

The extra calls cause unnecessary temporary strings to be constructed. Removing them makes the code smaller and faster.

额外的调用会导致构造不必要的临时字符串。移除它们可以使代码更小、更快。

```c++
a = absl::StrCat(a, b); // Use absl::StrAppend(&a, b) instead.
// 使用 absl::StrAppend(&a, b) 替代 absl::StrCat(a, b)。
```

Does not diagnose cases where absl::StrCat() is used as a template argument for a functor.

不会诊断将 absl::StrCat() 用作函数对象模板参数的情况。
