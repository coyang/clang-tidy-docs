# cppcoreguidelines-macro-usage

Finds macro usage that is considered problematic because better language constructs exist for the task.

查找被认为存在问题的宏使用方式，因为已有更好的语言结构可以完成相同的任务。

The relevant sections in the C++ Core Guidelines are [ES.31](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#es31-dont-use-macros-for-constants-or-functions), and [ES.32](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#es32-use-all_caps-for-all-macro-names).

相关的 C++ 核心指南章节包括 [ES.31](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#es31-dont-use-macros-for-constants-or-functions) 和 [ES.32](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#es32-use-all_caps-for-all-macro-names)。

Examples:

示例：

```c++
#define C 0
#define F1(x, y) ((a) > (b) ? (a) : (b))
#define F2(...) (__VA_ARGS__)
#define F3(x, y) x##y
#define COMMA ,
#define NORETURN [[noreturn]]
#define DEPRECATED attribute((deprecated))
#if LIB_EXPORTS
#define DLLEXPORTS __declspec(dllexport)
#else
#define DLLEXPORTS __declspec(dllimport)
#endif
```

results in the following warnings:

会产生如下警告：

    4 warnings generated.
    test.cpp:1:9: warning: macro 'C' used to declare a constant; consider using a 'constexpr' constant [cppcoreguidelines-macro-usage]
    #define C 0
            ^
    test.cpp:2:9: warning: function-like macro 'F1' used; consider a 'constexpr' template function [cppcoreguidelines-macro-usage]
    #define F1(x, y) ((a) > (b) ? (a) : (b))
            ^
    test.cpp:3:9: warning: variadic macro 'F2' used; consider using a 'constexpr' variadic template function [cppcoreguidelines-macro-usage]
    #define F2(...) (__VA_ARGS__)
            ^

## Options

## 选项

::: option
AllowedRegexp

A regular expression to filter allowed macros. For example [DEBUG*|LIBTORRENT*|TORRENT*|UNI*]{.title-ref} could be applied to filter [libtorrent]{.title-ref}. Default value is [^DEBUG_*]{.title-ref}.

用于筛选允许的宏的正则表达式。例如，可以使用 [DEBUG*|LIBTORRENT*|TORRENT*|UNI*]{.title-ref} 来过滤 [libtorrent]{.title-ref}。默认值为 [^DEBUG_*]{.title-ref}。

:::

::: option
CheckCapsOnly

Boolean flag to warn on all macros except those with CAPS_ONLY names. This option is intended to ease introduction of this check into older code bases. Default value is [false]{.title-ref}.

布尔标志，用于对除全大写命名的宏之外的所有宏发出警告。此选项旨在便于将此检查引入旧代码库。默认值为 [false]{.title-ref}。

:::

::: option
IgnoreCommandLineMacros

Boolean flag to toggle ignoring command-line-defined macros. Default value is [true]{.title-ref}.

布尔标志，用于切换是否忽略命令行定义的宏。默认值为 [true]{.title-ref}。

:::
