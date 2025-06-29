# bugprone-unsafe-functions

Checks for functions that have safer, more secure replacements available, or are considered deprecated due to design flaws. The check heavily relies on the functions from the Annex K. "Bounds-checking interfaces" of C11.

检查是否使用了存在更安全替代方案的函数，或因设计缺陷而被视为已弃用的函数。该检查主要依赖于 C11 标准中附录 K（Annex K）中定义的“边界检查接口”函数。

The check implements the following rules from the CERT C Coding Standard:

该检查实现了 CERT C 编码标准中的以下规则：

- Recommendation MSC24-C. Do not use deprecated or obsolescent functions  
  建议 MSC24-C：不要使用已弃用或过时的函数  
  https://wiki.sei.cmu.edu/confluence/display/c/MSC24-C.+Do+not+use+deprecated+or+obsolescent+functions

- Rule MSC33-C. Do not pass invalid data to the asctime() function  
  规则 MSC33-C：不要向 asctime() 函数传递无效数据  
  https://wiki.sei.cmu.edu/confluence/display/c/MSC33-C.+Do+not+pass+invalid+data+to+the+asctime%28%29+function

cert-msc24-c 和 cert-msc33-c 是该检查的别名。

cert-msc24-c 和 cert-msc33-c 是此检查的别名。

## Unsafe functions

不安全函数

The following functions are reported if ReportDefaultFunctions is enabled.

如果启用了 ReportDefaultFunctions 选项，将报告以下函数。

If Annex K. is available, a replacement from Annex K. is suggested for the following functions:

如果支持附录 K，将为以下函数建议使用附录 K 中的替代函数：

asctime, asctime_r, bsearch, ctime, fopen, fprintf,  
freopen, fscanf, fwprintf, fwscanf, getenv, gets, gmtime,  
localtime, mbsrtowcs, mbstowcs, memcpy, memmove, memset,  
printf, qsort, scanf, snprintf, sprintf, sscanf, strcat,  
strcpy, strerror, strlen, strncat, strncpy, strtok,  
swprintf, swscanf, vfprintf, vfscanf, vfwprintf, vfwscanf,  
vprintf, vscanf, vsnprintf, vsprintf, vsscanf, vswprintf,  
vswscanf, vwprintf, vwscanf, wcrtomb, wcscat, wcscpy,  
wcslen, wcsncat, wcsncpy, wcsrtombs, wcstok, wcstombs,  
wctomb, wmemcpy, wmemmove, wprintf, wscanf

If Annex K. is not available, replacements are suggested only for the following functions from the previous list:

如果不支持附录 K，仅对以下函数建议替代方案：

- asctime, asctime_r，建议替代：strftime
- gets，建议替代：fgets

The following functions are always checked, regardless of Annex K availability:

以下函数始终会被检查，无论是否支持附录 K：

- rewind，建议替代：fseek
- setbuf，建议替代：setvbuf

If ReportMoreUnsafeFunctions is enabled, the following functions are also checked:

如果启用了 ReportMoreUnsafeFunctions 选项，还将检查以下函数：

- bcmp，建议替代：memcmp
- bcopy，建议替代：若支持附录 K 使用 memcpy_s，否则使用 memcpy
- bzero，建议替代：若支持附录 K 使用 memset_s，否则使用 memset
- getpw，建议替代：getpwuid
- vfork，建议替代：posix_spawn

Although mentioned in the associated CERT rules, the following functions are ignored by the check:

尽管在 CERT 规则中提到，以下函数不会被此检查报告：

atof, atoi, atol, atoll, tmpfile

The availability of Annex K is determined based on the following macros:

是否支持附录 K 由以下宏定义决定：

- **STDC_LIB_EXT1**：特性宏，表示库实现中支持附录 K 的“边界检查接口”
- **STDC_WANT_LIB_EXT1**：用户定义的宏，表示用户请求定义附录 K 中的函数

Both macros have to be defined to suggest replacement functions from Annex K. **STDC_LIB_EXT1** is defined by the library implementation, and **STDC_WANT_LIB_EXT1** must be defined to 1 by the user before including any system headers.

必须同时定义这两个宏，才能启用附录 K 中的替代函数建议。**STDC_LIB_EXT1** 由库实现定义，**STDC_WANT_LIB_EXT1** 必须由用户在包含任何系统头文件之前定义为 1。

## Custom functions {#CustomFunctions}

自定义函数 {#CustomFunctions}

The option CustomFunctions allows the user to define custom functions to be checked. The format is the following, without newlines:

选项 CustomFunctions 允许用户定义需要检查的自定义函数。格式如下（不包含换行）：

```
bugprone-unsafe-functions.CustomFunctions="
  functionRegex1[, replacement1[, reason1]];
  functionRegex2[, replacement2[, reason2]];
  ...
"
```

The functions are matched using POSIX extended regular expressions.  
(Note: The regular expressions do not support negative (?! ) matches.)

函数名称使用 POSIX 扩展正则表达式进行匹配。  
（注意：正则表达式不支持负向匹配 (?! )。）

The reason is optional and is used to provide additional information about the reasoning behind the replacement. The default reason is is marked as unsafe.

reason（原因）字段是可选的，用于说明替代建议的理由。默认理由为“被标记为不安全”。

If replacement is empty, the text it should not be used will be shown instead of the suggestion for a replacement.

如果 replacement（替代函数）为空，将显示“该函数不应被使用”而不是替代建议。

As an example, the configuration ^original$, replacement, is deprecated; will produce the following diagnostic message.

例如，配置 ^original$, replacement, is deprecated; 将生成如下诊断信息：

```c
original(); // warning: function 'original' is deprecated; 'replacement' should be used instead.
::std::original(); // no-warning
original_function(); // no-warning
```

If the regular expression contains the character :, it is matched against the qualified name (i.e. std::original), otherwise the regex is matched against the unqualified name (original). If the regular expression starts with :: (or ^::), it is matched against the fully qualified name (::std::original).

如果正则表达式中包含字符 :，则会与限定名称（如 std::original）进行匹配；否则，将与未限定名称（如 original）匹配。如果正则表达式以 ::（或 ^::）开头，则与完全限定名称（如 ::std::original）匹配。

::: note  
注：

Fully qualified names can contain template parameters on certain C++ classes, but not on C++ functions. Type aliases are resolved before matching.

完全限定名称在某些 C++ 类中可以包含模板参数，但在 C++ 函数中不允许。类型别名在匹配前会被解析。

As an example, the member function open in the class std::ifstream has a fully qualified name of ::std::basic_ifstream<char>::open.

例如，类 std::ifstream 中的成员函数 open 的完全限定名称为 ::std::basic_ifstream<char>::open。

The example could also be matched with the regex ::std::basic_ifstream<[^>]\*>::open, which matches all potential template parameters, but does not match nested template classes.

该示例也可使用正则表达式 ::std::basic_ifstream<[^>]\*>::open 进行匹配，该表达式可匹配所有可能的模板参数，但不匹配嵌套模板类。
:::

## Options

选项

::: option  
ReportMoreUnsafeFunctions

When true, additional functions from widely used APIs (such as POSIX) are added to the list of reported functions. See the main documentation of the check for the complete list as to what this option enables. Default is true.

当设置为 true 时，将把来自广泛使用的 API（如 POSIX）中的附加函数加入报告列表。完整列表请参阅主文档。默认值为 true。
:::

::: option  
ReportDefaultFunctions

When true, the check reports the default set of functions. Consider changing the setting to false if you only want to see custom functions matched via custom functions. Default is true.

当设置为 true 时，检查将报告默认函数集。如果您只希望看到通过自定义函数匹配的函数，请考虑将此设置为 false。默认值为 true。
:::

::: option  
CustomFunctions

A semicolon-separated list of custom functions to be matched. A matched function contains a regular expression, an optional name of the replacement function, and an optional reason, separated by comma. For more information, see Custom functions.

以分号分隔的自定义函数列表。每个匹配项包含一个正则表达式、一个可选的替代函数名称和一个可选的原因，三者用逗号分隔。更多信息请参见“自定义函数”部分。
:::

## Examples

示例

```c++
#ifndef __STDC_LIB_EXT1__
#error "Annex K is not supported by the current standard library implementation."
#endif

#define __STDC_WANT_LIB_EXT1__ 1

#include <string.h> // Defines functions from Annex K.
#include <stdio.h>

enum { BUFSIZE = 32 };

void Unsafe(const char *Msg) {
  static const char Prefix[] = "Error: ";
  static const char Suffix[] = "\n";
  char Buf[BUFSIZE] = {0};

  strcpy(Buf, Prefix); // warning: function 'strcpy' is not bounds-checking; 'strcpy_s' should be used instead.
  strcat(Buf, Msg);    // warning: function 'strcat' is not bounds-checking; 'strcat_s' should be used instead.
  strcat(Buf, Suffix); // warning: function 'strcat' is not bounds-checking; 'strcat_s' should be used instead.
  if (fputs(buf, stderr) < 0) {
    // error handling
    return;
  }
}

void UsingSafeFunctions(const char *Msg) {
  static const char Prefix[] = "Error: ";
  static const char Suffix[] = "\n";
  char Buf[BUFSIZE] = {0};

  if (strcpy_s(Buf, BUFSIZE, Prefix) != 0) {
    // error handling
    return;
  }

  if (strcat_s(Buf, BUFSIZE, Msg) != 0) {
    // error handling
    return;
  }

  if (strcat_s(Buf, BUFSIZE, Suffix) != 0) {
    // error handling
    return;
  }

  if (fputs(Buf, stderr) < 0) {
    // error handling
    return;
  }
}
```

```c++
// 中文翻译版本

#ifndef __STDC_LIB_EXT1__
#error "当前标准库实现不支持附录 K。"
#endif

#define __STDC_WANT_LIB_EXT1__ 1

#include <string.h> // 定义附录 K 中的函数
#include <stdio.h>

enum { BUFSIZE = 32 };

void Unsafe(const char *Msg) {
  static const char Prefix[] = "Error: ";
  static const char Suffix[] = "\n";
  char Buf[BUFSIZE] = {0};

  strcpy(Buf, Prefix); // 警告：函数 'strcpy' 不是边界检查函数；应使用 'strcpy_s' 替代。
  strcat(Buf, Msg);    // 警告：函数 'strcat' 不是边界检查函数；应使用 'strcat_s' 替代。
  strcat(Buf, Suffix); // 警告：函数 'strcat' 不是边界检查函数；应使用 'strcat_s' 替代。
  if (fputs(buf, stderr) < 0) {
    // 错误处理
    return;
  }
}

void UsingSafeFunctions(const char *Msg) {
  static const char Prefix[] = "Error: ";
  static const char Suffix[] = "\n";
  char Buf[BUFSIZE] = {0};

  if (strcpy_s(Buf, BUFSIZE, Prefix) != 0) {
    // 错误处理
    return;
  }

  if (strcat_s(Buf, BUFSIZE, Msg) != 0) {
    // 错误处理
    return;
  }

  if (strcat_s(Buf, BUFSIZE, Suffix) != 0) {
    // 错误处理
    return;
  }

  if (fputs(Buf, stderr) < 0) {
    // 错误处理
    return;
  }
}
```
