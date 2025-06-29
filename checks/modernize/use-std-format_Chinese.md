以下是翻译后的 markdown 文件内容，保留了中英文双语，中文在英文下方，并以一个空行分隔：

# modernize-use-std-format

Converts calls to absl::StrFormat, or other functions via configuration options, to C++20's std::format, or another function via a configuration option, modifying the format string appropriately and removing now-unnecessary calls to std::string::c_str() and std::string::data().

将对 absl::StrFormat 的调用（或通过配置选项指定的其他函数）转换为 C++20 的 std::format（或通过配置选项指定的其他函数），适当修改格式字符串，并移除对 std::string::c_str() 和 std::string::data() 的不再必要的调用。

For example, it turns lines like

例如，它会将如下代码：

```c++
return absl::StrFormat("The %s is %3d", description.c_str(), value);
```

into:

转换为：

```c++
return std::format("The {} is {:3}", description, value);
```

The check uses the same format-string-conversion algorithm as modernize-use-std-print and its shortcomings and behaviour in combination with macros are described in the documentation for that check.

该检查使用与 modernize-use-std-print 相同的格式字符串转换算法，其局限性以及与宏结合使用时的行为在该检查的文档中有详细说明。

## Options

## 选项

::: option
StrictMode

When true, the check will add casts when converting from variadic functions and printing signed or unsigned integer types (including fixed-width integer types from <cstdint>, ptrdiff_t, size_t and ssize_t) as the opposite signedness to ensure that the output would matches that of a simple wrapper for std::sprintf that accepted a C-style variable argument list. For example, with StrictMode enabled,

当为 true 时，在从可变参数函数转换并打印有符号或无符号整数类型（包括来自 <cstdint> 的定宽整数类型、ptrdiff_t、size_t 和 ssize_t）时，检查器会添加类型转换，以相反的符号类型进行打印，以确保输出与一个简单封装的 std::sprintf（接受 C 风格可变参数列表）的行为一致。例如，当启用 StrictMode 时：

```c++
extern std::string strprintf(const char *format, ...);
int i = -42;
unsigned int u = 0xffffffff;
return strprintf("%u %d\n", i, u);
```

would be converted to

将被转换为：

```c++
return std::format("{} {}\n", static_cast<unsigned int>(i), static_cast<int>(u));
```

to ensure that the output will continue to be the unsigned representation of -42 and the signed representation of 0xffffffff (often 4294967254 and -1 respectively). When false (which is the default), these casts will not be added which may cause a change in the output. Note that this option makes no difference for the default value of StrFormatLikeFunctions since absl::StrFormat takes a function parameter pack and is not a variadic function.

以确保输出仍然是 -42 的无符号表示和 0xffffffff 的有符号表示（通常分别为 4294967254 和 -1）。当为 false（默认值）时，将不会添加这些类型转换，这可能会导致输出发生变化。请注意，对于 StrFormatLikeFunctions 的默认值，此选项没有影响，因为 absl::StrFormat 接受的是函数参数包，而不是可变参数函数。
:::

::: option
StrFormatLikeFunctions

A semicolon-separated list of (fully qualified) function names to replace, with the requirement that the first parameter contains the printf-style format string and the arguments to be formatted follow immediately afterwards. Qualified member function names are supported, but the replacement function name must be unqualified. The default value for this option is absl::StrFormat.

一个以分号分隔的（完全限定）函数名列表，用于指定要替换的函数。要求第一个参数为 printf 风格的格式字符串，后续参数为要格式化的内容。支持限定的成员函数名，但替换函数名必须是不带限定的。该选项的默认值为 absl::StrFormat。
:::

::: option
ReplacementFormatFunction

The function that will be used to replace the function set by the StrFormatLikeFunctions option rather than the default std::format. It is expected that the function provides an interface that is compatible with std::format. A suitable candidate would be fmt::format.

用于替换 StrFormatLikeFunctions 选项中指定函数的函数，而不是默认的 std::format。该函数应提供与 std::format 兼容的接口。一个合适的候选函数是 fmt::format。
:::

::: option
FormatHeader

The header that must be included for the declaration of ReplacementFormatFunction so that a #include directive can be added if required. If ReplacementFormatFunction is std::format then this option will default to <format>, otherwise this option will default to nothing and no #include directive will be added.

用于声明 ReplacementFormatFunction 所需包含的头文件，以便在需要时添加 #include 指令。如果 ReplacementFormatFunction 是 std::format，则该选项默认为 <format>；否则，该选项默认为空，不会添加任何 #include 指令。
:::
