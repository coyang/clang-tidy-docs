# bugprone-suspicious-string-compare

Find suspicious usage of runtime string comparison functions. This check is valid in C and C++.

查找运行时字符串比较函数的可疑用法。此检查适用于 C 和 C++。

Checks for calls with implicit comparator and proposed to explicitly add it.

检查使用隐式比较器的调用，并建议显式添加比较器。

```c++
if (strcmp(...))       // Implicitly compare to zero
if (!strcmp(...))      // Won't warn
if (strcmp(...) != 0)  // Won't warn
```

```c++
if (strcmp(...))       // 隐式与零比较
if (!strcmp(...))      // 不会发出警告
if (strcmp(...) != 0)  // 不会发出警告
```

Checks that compare function results (i.e., strcmp) are compared to valid constant. The resulting value is

检查比较函数的结果（例如 strcmp）是否与有效常量进行比较。其返回值含义如下：

```
<  0    when lower than,
>  0    when greater than,
== 0    when equals.
```

```
<  0    表示小于，
>  0    表示大于，
== 0    表示相等。
```

A common mistake is to compare the result to 1 or -1.

一个常见错误是将结果与 1 或 -1 进行比较。

```c++
if (strcmp(...) == -1)  // Incorrect usage of the returned value.
```

```c++
if (strcmp(...) == -1)  // 错误地使用了返回值。
```

Additionally, the check warns if the results value is implicitly cast to a suspicious non-integer type. It's happening when the returned value is used in a wrong context.

此外，如果返回值被隐式转换为可疑的非整数类型，该检查也会发出警告。这通常发生在返回值被用于错误的上下文中。

```c++
if (strcmp(...) < 0.)  // Incorrect usage of the returned value.
```

```c++
if (strcmp(...) < 0.)  // 错误地使用了返回值。
```

## Options

## 选项

::: option
WarnOnImplicitComparison

When true, the check will warn on implicit comparison. true by default.

当为 true 时，该检查会对隐式比较发出警告。默认值为 true。
:::

::: option
WarnOnLogicalNotComparison

When true, the check will warn on logical not comparison. false by default.

当为 true 时，该检查会对逻辑非比较发出警告。默认值为 false。
:::

::: option
StringCompareLikeFunctions

A string specifying the comma-separated names of the extra string comparison functions. Default is an empty string. The check will detect the following string comparison functions:
**builtin_memcmp, **builtin_strcasecmp, **builtin_strcmp, **builtin_strncasecmp, \_\_builtin_strncmp, \_mbscmp, \_mbscmp_l, \_mbsicmp, \_mbsicmp_l, \_mbsnbcmp, \_mbsnbcmp_l, \_mbsnbicmp, \_mbsnbicmp_l, \_mbsncmp, \_mbsncmp_l, \_mbsnicmp, \_mbsnicmp_l, \_memicmp, \_memicmp_l, \_stricmp, \_stricmp_l, \_strnicmp, \_strnicmp_l, \_wcsicmp, \_wcsicmp_l, \_wcsnicmp, \_wcsnicmp_l, lstrcmp, lstrcmpi, memcmp, memicmp, strcasecmp, strcmp, strcmpi, stricmp, strncasecmp, strncmp, strnicmp, wcscasecmp, wcscmp, wcsicmp, wcsncmp, wcsnicmp, wmemcmp.

一个字符串，指定额外字符串比较函数的名称，名称之间用逗号分隔。默认值为空字符串。该检查将检测以下字符串比较函数：
**builtin_memcmp、**builtin_strcasecmp、**builtin_strcmp、**builtin_strncasecmp、\_\_builtin_strncmp、\_mbscmp、\_mbscmp_l、\_mbsicmp、\_mbsicmp_l、\_mbsnbcmp、\_mbsnbcmp_l、\_mbsnbicmp、\_mbsnbicmp_l、\_mbsncmp、\_mbsncmp_l、\_mbsnicmp、\_mbsnicmp_l、\_memicmp、\_memicmp_l、\_stricmp、\_stricmp_l、\_strnicmp、\_strnicmp_l、\_wcsicmp、\_wcsicmp_l、\_wcsnicmp、\_wcsnicmp_l、lstrcmp、lstrcmpi、memcmp、memicmp、strcasecmp、strcmp、strcmpi、stricmp、strncasecmp、strncmp、strnicmp、wcscasecmp、wcscmp、wcsicmp、wcsncmp、wcsnicmp、wmemcmp。
:::
