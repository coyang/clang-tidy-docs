# modernize-deprecated-headers

一些来自 C 标准库的头文件在 C++ 中已被弃用，不再推荐在 C++ 代码库中使用。其中一些在 C++ 中没有任何作用。更多详情请参考 C++14 标准的 [depr.c.headers] 部分。

Some headers from C library were deprecated in C++ and are no longer welcome in C++ codebases. Some have no effect in C++. For more details refer to the C++14 Standard [depr.c.headers] section.

此检查会将 C 标准库头文件替换为其 C++ 替代版本，并移除冗余的头文件。

This check replaces C standard library headers with their C++ alternatives and removes redundant ones.

```c++
// C++ source file...
#include <assert.h>
#include <stdbool.h>

// becomes

#include <cassert>
// No 'stdbool.h' here.
```

重要提示：标准并不保证 C++ 头文件会在全局命名空间中声明所有相同的函数。当前形式下的检查可能会破坏使用全局命名空间中库符号的代码。

Important note: the Standard doesn't guarantee that the C++ headers declare all the same functions in the global namespace. The check in its current form can break the code that uses library symbols from the global namespace.

- <assert.h>
- <complex.h>
- <ctype.h>
- <errno.h>
- <fenv.h> // 自 C++11 起弃用
- <float.h>
- <inttypes.h>
- <limits.h>
- <locale.h>
- <math.h>
- <setjmp.h>
- <signal.h>
- <stdarg.h>
- <stddef.h>
- <stdint.h>
- <stdio.h>
- <stdlib.h>
- <string.h>
- <tgmath.h> // 自 C++11 起弃用
- <time.h>
- <uchar.h> // 自 C++11 起弃用
- <wchar.h>
- <wctype.h>

- <assert.h>
- <complex.h>
- <ctype.h>
- <errno.h>
- <fenv.h> // deprecated since C++11
- <float.h>
- <inttypes.h>
- <limits.h>
- <locale.h>
- <math.h>
- <setjmp.h>
- <signal.h>
- <stdarg.h>
- <stddef.h>
- <stdint.h>
- <stdio.h>
- <stdlib.h>
- <string.h>
- <tgmath.h> // deprecated since C++11
- <time.h>
- <uchar.h> // deprecated since C++11
- <wchar.h>
- <wctype.h>

如果指定的标准早于 C++11，该检查器只会替换在 C++11 之前已被弃用的头文件；否则，将替换上面列表中出现的所有头文件。

If the specified standard is older than C++11 the check will only replace headers deprecated before C++11, otherwise -- every header that appeared in the previous list.

以下这些头文件在 C++ 中没有任何作用：

These headers don't have effect in C++:

- <iso646.h>
- <stdalign.h>
- <stdbool.h>

- <iso646.h>
- <stdalign.h>
- <stdbool.h>

该检查器会忽略 extern "C" { ... } 块中的 include 指令，因为库可能希望为 C 和 C++ 提供某些 API。

The checker ignores include directives within extern "C" { ... } blocks, since a library might want to expose some API for C and C++ libraries.

```c++
// C++ source file...
extern "C" {
#include <assert.h>  // 保留不变。
#include <stdbool.h> // 保留不变。
}
```

```c++
// C++ source file...
extern "C" {
#include <assert.h>  // Left intact.
#include <stdbool.h> // Left intact.
}
```

## 选项

## Options

::: option
CheckHeaderFile

[clang-tidy] 无法判断当前分析的 C++ 源文件所包含的头文件是否也被其他 C 源文件包含。因此，为了避免误报和错误的修复建议，我们默认不在头文件中生成报告。如果确定项目中的头文件仅被 C++ 源文件使用，可以将此选项设置为 true。默认值为 false。

[clang-tidy] cannot know if the header file included by the currently analyzed C++ source file is not included by any other C source files. Hence, to omit false-positives and wrong fixit-hints, we ignore emitting reports into header files. One can set this option to true if they know that the header files in the project are only used by C++ source files. Default is false.
:::
