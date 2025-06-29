# abseil-string-find-startswith

Checks whether a std::string::find() or std::string::rfind() (and corresponding std::string_view methods) result is compared with 0, and suggests replacing with absl::StartsWith(). This is both a readability and performance issue.

检查是否将 std::string::find() 或 std::string::rfind()（以及相应的 std::string_view 方法）的结果与 0 进行比较，并建议使用 absl::StartsWith() 进行替换。这既是可读性问题，也是性能问题。

starts_with was added as a built-in function on those types in C++20. If available, prefer enabling modernize-use-starts-ends-with instead of this check.

从 C++20 开始，starts_with 被添加为这些类型的内建函数。如果可用，建议启用 modernize-use-starts-ends-with 检查，而不是使用本检查。

```c++
string s = "...";
if (s.find("Hello World") == 0) { /* do something */ }
if (s.rfind("Hello World", 0) == 0) { /* do something */ }
```

```c++
string s = "...";
if (absl::StartsWith(s, "Hello World")) { /* do something */ }
if (absl::StartsWith(s, "Hello World")) { /* do something */ }
```

## Options

## 选项

::: option
StringLikeClasses

Semicolon-separated list of names of string-like classes. By default both std::basic_string and std::basic_string_view are considered. The list of methods to be considered is fixed.

以分号分隔的类名列表，用于指定类字符串类型。默认情况下，会考虑 std::basic_string 和 std::basic_string_view。要检查的方法列表是固定的。
:::

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用的 include 风格，可选值为 llvm 或 google。默认值为 llvm。
:::

::: option
AbseilStringsMatchHeader

The location of Abseil's strings/match.h. Defaults to absl/strings/match.h.

Abseil 的 strings/match.h 文件的位置。默认值为 absl/strings/match.h。
:::
