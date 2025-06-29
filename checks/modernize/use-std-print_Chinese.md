# modernize-use-std-print

Converts calls to printf, fprintf, absl::PrintF and absl::FPrintf to equivalent calls to C++23's std::print or std::println as appropriate, modifying the format string appropriately. The replaced and replacement functions can be customised by configuration options. Each argument that is the result of a call to std::string::c_str() and std::string::data() will have that now-unnecessary call removed in a similar manner to the readability-redundant-string-cstr check.

将对 printf、fprintf、absl::PrintF 和 absl::FPrintf 的调用转换为 C++23 中等效的 std::print 或 std::println 调用，并相应地修改格式字符串。被替换和替代的函数可以通过配置选项进行自定义。每个由 std::string::c_str() 或 std::string::data() 调用产生的参数，其不再必要的调用将被移除，方式类似于 readability-redundant-string-cstr 检查。

In other words, it turns lines like:

换句话说，它会将如下代码：

```c++
fprintf(stderr, "The %s is %3d\n", description.c_str(), value);
```

into:

转换为：

```c++
std::println(stderr, "The {} is {:3}", description, value);
```

If the ReplacementPrintFunction or ReplacementPrintlnFunction options are left at or set to their default values then this check is only enabled with -std=c++23 or later.

如果 ReplacementPrintFunction 或 ReplacementPrintlnFunction 选项保持默认值或未设置，则此检查仅在使用 -std=c++23 或更高版本时启用。

Macros starting with PRI and \_\_PRI from <inttypes.h> are expanded, escaping is handled and adjacent strings are concatenated to form a single StringLiteral before the format string is converted. Use of any other macros in the format string will cause a warning message to be emitted and no conversion will be performed. The converted format string will always be a single string literal.

来自 <inttypes.h> 的以 PRI 和 \_\_PRI 开头的宏会被展开，转义字符会被正确处理，相邻的字符串会被合并为一个 StringLiteral，然后再对格式字符串进行转换。如果格式字符串中使用了其他宏，将会发出警告信息，并且不会进行转换。转换后的格式字符串始终是一个单独的字符串字面量。

The check doesn't do a bad job, but it's not perfect. In particular:

该检查效果尚可，但并不完美。特别是：

- It assumes that the format string is correct for the arguments. If you get any warnings when compiling with -Wformat then misbehaviour is possible.

  它假设格式字符串与参数匹配。如果在使用 -Wformat 编译时出现任何警告，则可能出现错误行为。

- At the point that the check runs, the AST contains a single StringLiteral for the format string where escapes have been expanded. The check tries to reconstruct escape sequences, they may not be the same as they were written (e.g. "\x41\x0a" will become "A\n" and "ab" "cd" will become "abcd".)

  在检查运行时，AST 中的格式字符串是一个已展开转义字符的 StringLiteral。检查器会尝试重建转义序列，但可能与原始写法不同（例如 "\x41\x0a" 会变成 "A\n"，而 "ab" "cd" 会变成 "abcd"）。

- It supports field widths, precision, positional arguments, leading zeros, leading +, alignment and alternative forms.

  它支持字段宽度、精度、位置参数、前导零、前导 +、对齐方式和替代格式。

- Use of any unsupported flags or specifiers will cause the entire statement to be left alone and a warning to be emitted. Particular unsupported features are:

  使用任何不支持的标志或说明符将导致整个语句保持不变，并发出警告。特别是不支持的功能包括：
  - The %' flag for thousands separators.

    用于千位分隔符的 %' 标志。

  - The glibc extension %m.

    glibc 扩展的 %m。

- printf and similar functions return the number of characters printed. std::print does not. This means that any invocations that use the return value will not be converted. Unfortunately this currently includes explicitly-casting to void. Deficiencies in this check mean that any invocations inside GCC compound statements cannot be converted even if the resulting value is not used.

  printf 及类似函数会返回打印的字符数，而 std::print 不会。因此，任何使用返回值的调用都不会被转换。不幸的是，这目前包括显式转换为 void 的情况。由于此检查的不足，即使结果值未被使用，GCC 复合语句中的调用也无法被转换。

If conversion would be incomplete or unsafe then the entire invocation will be left unchanged.

如果转换不完整或不安全，则整个调用将保持不变。

If the call is deemed suitable for conversion then:

如果调用被认为适合转换，则：

- printf、fprintf、absl::PrintF、absl::FPrintF 以及由 PrintfLikeFunctions 或 FprintfLikeFunctions 选项指定的任何函数将被替换为 ReplacementPrintlnFunction 指定的函数（如果格式字符串以 \n 结尾）或 ReplacementPrintFunction 指定的函数（否则）。

- the format string is rewritten to use the std::formatter language. If a \n is found at the end of the format string not preceded by r then it is removed and ReplacementPrintlnFunction is used rather than ReplacementPrintFunction.

  格式字符串将被重写为使用 std::formatter 语法。如果格式字符串末尾包含 \n 且前面没有 r，则该 \n 会被移除，并使用 ReplacementPrintlnFunction 而非 ReplacementPrintFunction。

- any arguments that corresponded to %p specifiers that std::formatter wouldn't accept are wrapped in a static_cast to const void \*.

  与 %p 说明符对应但 std::formatter 不接受的参数将被包装为 static_cast<const void \*>。

- any arguments that corresponded to %s specifiers where the argument is of signed char or unsigned char type are wrapped in a reinterpret_cast<const char \*>.

  与 %s 说明符对应且参数类型为 signed char 或 unsigned char 的参数将被包装为 reinterpret_cast<const char \*>。

- any arguments where the format string and the parameter differ in signedness will be wrapped in an appropriate static_cast if StrictMode is enabled.

  如果启用了 StrictMode，格式字符串与参数符号性不一致的参数将被包装为适当的 static_cast。

- any arguments that end in a call to std::string::c_str() or std::string::data() will have that call removed.

  任何以 std::string::c_str() 或 std::string::data() 调用结尾的参数，其调用将被移除。

## Options

## 选项

::: option
StrictMode

When true, the check will add casts when converting from variadic functions like printf and printing signed or unsigned integer types (including fixed-width integer types from <cstdint>, ptrdiff_t, size_t and ssize_t) as the opposite signedness to ensure that the output matches that of printf. This does not apply when converting from non-variadic functions such as absl::PrintF and fmt::printf. For example, with StrictMode enabled:

当为 true 时，在从 printf 等可变参数函数转换并打印有符号或无符号整数类型（包括来自 <cstdint> 的定宽整数类型、ptrdiff_t、size_t 和 ssize_t）时，检查器将添加类型转换为相反的符号性，以确保输出与 printf 一致。当从 absl::PrintF 或 fmt::printf 等非可变参数函数转换时不适用。例如，在启用 StrictMode 时：

```c++
int i = -42;
unsigned int u = 0xffffffff;
printf("%u %d\n", i, u);
```

would be converted to:

将被转换为：

```c++
std::print("{} {}\n", static_cast<unsigned int>(i), static_cast<int>(u));
```

to ensure that the output will continue to be the unsigned representation of -42 and the signed representation of 0xffffffff (often 4294967254 and -1 respectively.) When false (which is the default), these casts will not be added which may cause a change in the output.

以确保输出仍然是 -42 的无符号表示和 0xffffffff 的有符号表示（通常分别为 4294967254 和 -1）。当为 false（默认值）时，不会添加这些类型转换，可能导致输出发生变化。
:::

::: option
PrintfLikeFunctions

A semicolon-separated list of (fully qualified) function names to replace, with the requirement that the first parameter contains the printf-style format string and the arguments to be formatted follow immediately afterwards. Qualified member function names are supported, but the replacement function name must be unqualified. If neither this option nor FprintfLikeFunctions are set then the default value for this option is printf; absl::PrintF, otherwise it is empty.

一个以分号分隔的（完全限定）函数名列表，用于替换函数。要求第一个参数为 printf 风格的格式字符串，后续参数为要格式化的内容。支持限定的成员函数名，但替代函数名必须是非限定的。如果未设置此选项或 FprintfLikeFunctions，则默认值为 printf; absl::PrintF，否则为空。
:::

::: option
FprintfLikeFunctions

A semicolon-separated list of (fully qualified) function names to replace, with the requirement that the first parameter is retained, the second parameter contains the printf-style format string and the arguments to be formatted follow immediately afterwards. Qualified member function names are supported, but the replacement function name must be unqualified. If neither this option nor PrintfLikeFunctions are set then the default value for this option is fprintf; absl::FPrintF, otherwise it is empty.

一个以分号分隔的（完全限定）函数名列表，用于替换函数。要求保留第一个参数，第二个参数为 printf 风格的格式字符串，后续参数为要格式化的内容。支持限定的成员函数名，但替代函数名必须是非限定的。如果未设置此选项或 PrintfLikeFunctions，则默认值为 fprintf; absl::FPrintF，否则为空。
:::

::: option
ReplacementPrintFunction

The function that will be used to replace printf, fprintf etc. during conversion rather than the default std::print when the original format string does not end with \n. It is expected that the function provides an interface that is compatible with std::print. A suitable candidate would be fmt::print.

用于在转换过程中替代 printf、fprintf 等函数的函数（当原始格式字符串不以 \n 结尾时），而不是默认的 std::print。要求该函数提供与 std::print 兼容的接口。一个合适的候选函数是 fmt::print。
:::

::: option
ReplacementPrintlnFunction

The function that will be used to replace printf, fprintf etc. during conversion rather than the default std::println when the original format string ends with \n. It is expected that the function provides an interface that is compatible with std::println. A suitable candidate would be fmt::println.

用于在转换过程中替代 printf、fprintf 等函数的函数（当原始格式字符串以 \n 结尾时），而不是默认的 std::println。要求该函数提供与 std::println 兼容的接口。一个合适的候选函数是 fmt::println。
:::

::: option
PrintHeader

The header that must be included for the declaration of ReplacementPrintFunction so that a #include directive can be added if required. If ReplacementPrintFunction is std::print then this option will default to <print>, otherwise this option will default to nothing and no #include directive will be added.

用于声明 ReplacementPrintFunction 所需包含的头文件，以便在需要时添加 #include 指令。如果 ReplacementPrintFunction 是 std::print，则此选项默认为 <print>，否则默认为空，不会添加任何 #include 指令。
:::
