# llvmlibc-restrict-system-libc-headers

Finds includes of system libc headers not provided by the compiler within llvm-libc implementations.

查找在 llvm-libc 实现中包含的、编译器未提供的系统 libc 头文件。

```c++
#include <stdio.h>            // Not allowed because it is part of system libc.
                              // 不允许，因为它是系统 libc 的一部分。
#include <stddef.h>           // Allowed because it is provided by the compiler.
                              // 允许，因为它由编译器提供。
#include "internal/stdio.h"   // Allowed because it is NOT part of system libc.
                              // 允许，因为它不是系统 libc 的一部分。
```

This check is necessary because accidentally including system libc headers can lead to subtle and hard to detect bugs. For example consider a system libc whose dirent struct has slightly different field ordering than llvm-libc. While this will compile successfully, this can cause issues during runtime because they are ABI incompatible.

进行此检查是必要的，因为意外地包含系统 libc 头文件可能会导致微妙且难以发现的错误。例如，考虑一个系统 libc 的 dirent 结构体，其字段顺序与 llvm-libc 略有不同。虽然这可以成功编译，但在运行时可能会出现问题，因为它们的 ABI 不兼容。

## Options

## 选项

::: option
Includes

A string containing a comma separated glob list of allowed include filenames. Similar to the -checks glob list for running clang-tidy itself, the two wildcard characters are _ and -, to include and exclude globs, respectively. The default is -_, which disallows all includes.

包含一个以逗号分隔的通配符列表字符串，表示允许包含的头文件名。类似于运行 clang-tidy 时使用的 -checks 通配符列表，两个通配符分别是 _（用于包含）和 -（用于排除）。默认值为 -_，表示禁止所有包含。

This can be used to allow known safe includes such as Linux development headers. See portability-restrict-system-includes for more details.

这可用于允许已知安全的包含文件，例如 Linux 开发头文件。有关更多详细信息，请参阅 portability-restrict-system-includes。  
:::
