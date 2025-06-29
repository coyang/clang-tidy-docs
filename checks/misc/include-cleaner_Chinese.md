# misc-include-cleaner

Checks for unused and missing includes. Generates findings only for the  
main file of a translation unit. Findings correspond to  
<https://clangd.llvm.org/design/include-cleaner>.

检查未使用和缺失的头文件引用。仅对翻译单元的主文件生成检查结果。  
检查结果遵循 <https://clangd.llvm.org/design/include-cleaner> 的规范。

Example:  
示例：

```c++
// foo.h
class Foo{};
// bar.h
#include "baz.h"
class Bar{};
// baz.h
class Baz{};
// main.cc
#include "bar.h" // OK: uses class Bar from bar.h
#include "foo.h" // warning: unused include "foo.h"
Bar bar;
Baz baz; // warning: missing include "baz.h"
```

## Options

## 选项

::: option  
IgnoreHeaders

A semicolon-separated list of regexes to disable insertion/removal of  
header files that match this regex as a suffix. E.g.,  
[foo/.*]{.title-ref} disables insertion/removal for all headers under  
the directory [foo]{.title-ref}. Default is an empty string, no headers  
will be ignored.

一个以分号分隔的正则表达式列表，用于禁用对匹配这些正则表达式后缀的头文件的插入或移除。  
例如，[foo/.*]{.title-ref} 会禁用对目录 [foo]{.title-ref} 下所有头文件的插入或移除。  
默认值为空字符串，表示不会忽略任何头文件。
:::

::: option  
DeduplicateFindings

A boolean that controls whether the check should deduplicate findings  
for the same symbol. Defaults to [true]{.title-ref}.

一个布尔值，用于控制是否对相同符号的检查结果进行去重。默认值为 [true]{.title-ref}。
:::

::: option  
UnusedIncludes

A boolean that controls whether the check should report unused includes  
(includes that are not used directly). Defaults to [true]{.title-ref}.

一个布尔值，用于控制是否报告未使用的头文件引用（即未被直接使用的引用）。默认值为 [true]{.title-ref}。
:::

::: option  
MissingIncludes

A boolean that controls whether the check should report missing includes  
(header files from which symbols are used but which are not directly  
included). Defaults to [true]{.title-ref}.

一个布尔值，用于控制是否报告缺失的头文件引用（即使用了其中符号但未直接包含的头文件）。默认值为 [true]{.title-ref}。
:::
