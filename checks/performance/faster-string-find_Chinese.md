# performance-faster-string-find

Optimize calls to std::string::find() and friends when the needle passed is a single character string literal. The character literal overload is more efficient.

优化对 std::string::find() 及其相关函数的调用，当传入的是单字符字符串字面量时。使用字符字面量的重载版本效率更高。

Examples:

示例：

```c++
str.find("A");

// becomes
// 变为

str.find('A');
```

## Options

## 选项

::: option
StringLikeClasses

Semicolon-separated list of names of string-like classes. By default only ::std::basic_string and ::std::basic_string_view are considered. The check will only consider member functions named find, rfind, find_first_of, find_first_not_of, find_last_of, or find_last_not_of within these classes.

以分号分隔的类名列表，表示类似字符串的类。默认情况下，仅考虑 ::std::basic_string 和 ::std::basic_string_view。此检查仅会考虑这些类中名为 find、rfind、find_first_of、find_first_not_of、find_last_of 或 find_last_not_of 的成员函数。
:::
