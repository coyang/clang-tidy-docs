# abseil-string-find-str-contains

Finds s.find(...) == string::npos comparisons (for various string-like types) and suggests replacing with absl::StrContains().

查找各种类似字符串类型中 s.find(...) == string::npos 的比较，并建议替换为 absl::StrContains()。

This improves readability and reduces the likelihood of accidentally mixing find() and npos from different string-like types.

这样可以提高代码可读性，并减少不小心混用来自不同字符串类型的 find() 和 npos 的可能性。

By default, "string-like types" includes ::std::basic_string, ::std::basic_string_view, and ::absl::string_view. See the StringLikeClasses option to change this.

默认情况下，“类似字符串类型”包括 ::std::basic_string、::std::basic_string_view 和 ::absl::string_view。可通过 StringLikeClasses 选项进行更改。

```c++
std::string s = "...";
if (s.find("Hello World") == std::string::npos) { /* do something */ }

absl::string_view a = "...";
if (absl::string_view::npos != a.find("Hello World")) { /* do something */ }
```

变为

```c++
std::string s = "...";
if (!absl::StrContains(s, "Hello World")) { /* do something */ }

absl::string_view a = "...";
if (absl::StrContains(a, "Hello World")) { /* do something */ }
```

## Options

## 选项

::: option
StringLikeClasses

Semicolon-separated list of names of string-like classes. By default includes ::std::basic_string, ::std::basic_string_view, and ::absl::string_view.

以分号分隔的类似字符串类名称列表。默认包括 ::std::basic_string、::std::basic_string_view 和 ::absl::string_view。
:::

::: option
IncludeStyle

A string specifying which include-style is used, llvm or google. Default is llvm.

一个字符串，用于指定使用哪种 include 风格，llvm 或 google。默认值为 llvm。
:::

::: option
AbseilStringsMatchHeader

The location of Abseil's strings/match.h. Defaults to absl/strings/match.h.

Abseil 的 strings/match.h 文件的位置。默认值为 absl/strings/match.h。
:::
