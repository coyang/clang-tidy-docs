# portability-restrict-system-includes

Checks to selectively allow or disallow a configurable list of system headers.

检查以选择性地允许或禁止一组可配置的系统头文件。

For example:

例如：

In order to only allow zlib.h from the system you would set the options to [-*,zlib.h].

为了仅允许系统中的 zlib.h，你可以将选项设置为 [-*,zlib.h]。

```c++
#include <curses.h>       // Bad: disallowed system header.
#include <openssl/ssl.h>  // Bad: disallowed system header.
#include <zlib.h>         // Good: allowed system header.
#include "src/myfile.h"   // Good: non-system header always allowed.

#include <curses.h>       // 错误：不允许的系统头文件。
#include <openssl/ssl.h>  // 错误：不允许的系统头文件。
#include <zlib.h>         // 正确：允许的系统头文件。
#include "src/myfile.h"   // 正确：非系统头文件始终允许。
```

In order to allow everything except zlib.h from the system you would set the options to [*,-zlib.h].

为了允许系统中除 zlib.h 之外的所有头文件，你可以将选项设置为 [*,-zlib.h]。

```c++
#include <curses.h>       // Good: allowed system header.
#include <openssl/ssl.h>  // Good: allowed system header.
#include <zlib.h>         // Bad: disallowed system header.
#include "src/myfile.h"   // Good: non-system header always allowed.

#include <curses.h>       // 正确：允许的系统头文件。
#include <openssl/ssl.h>  // 正确：允许的系统头文件。
#include <zlib.h>         // 错误：不允许的系统头文件。
#include "src/myfile.h"   // 正确：非系统头文件始终允许。
```

Since the options support globbing you can use wildcarding to allow groups of headers.

由于选项支持通配符匹配，你可以使用通配符来允许一组头文件。

[-*,openssl/*.h] will allow all openssl headers but disallow any others.

[-*,openssl/*.h] 将允许所有 openssl 头文件，但禁止其他所有头文件。

```c++
#include <curses.h>       // Bad: disallowed system header.
#include <openssl/ssl.h>  // Good: allowed system header.
#include <openssl/rsa.h>  // Good: allowed system header.
#include <zlib.h>         // Bad: disallowed system header.
#include "src/myfile.h"   // Good: non-system header always allowed.

#include <curses.h>       // 错误：不允许的系统头文件。
#include <openssl/ssl.h>  // 正确：允许的系统头文件。
#include <openssl/rsa.h>  // 正确：允许的系统头文件。
#include <zlib.h>         // 错误：不允许的系统头文件。
#include "src/myfile.h"   // 正确：非系统头文件始终允许。
```

## Options

## 选项

::: option
Includes

A string containing a comma separated glob list of allowed include filenames. Similar to the -checks glob list for running clang-tidy itself, the two wildcard characters are _ and -, to include and exclude globs, respectively. The default is _, which allows all includes.

一个字符串，包含以逗号分隔的允许包含的文件名的通配符列表。类似于运行 clang-tidy 时使用的 -checks 通配符列表，其中两个通配符分别是 _（包含）和 -（排除）。默认值为 _，表示允许所有包含的头文件。
:::
