# portability-restrict-system-includes

Checks to selectively allow or disallow a configurable list of system
headers.

For example:

In order to **only** allow [zlib.h]{.title-ref} from the system you
would set the options to [-\*,zlib.h]{.title-ref}.

```c++
#include <curses.h>       // Bad: disallowed system header.
#include <openssl/ssl.h>  // Bad: disallowed system header.
#include <zlib.h>         // Good: allowed system header.
#include "src/myfile.h"   // Good: non-system header always allowed.
```

In order to allow everything **except** [zlib.h]{.title-ref} from the
system you would set the options to [\*,-zlib.h]{.title-ref}.

```c++
#include <curses.h>       // Good: allowed system header.
#include <openssl/ssl.h>  // Good: allowed system header.
#include <zlib.h>         // Bad: disallowed system header.
#include "src/myfile.h"   // Good: non-system header always allowed.
```

Since the options support globbing you can use wildcarding to allow
groups of headers.

[-\*,openssl/\*.h]{.title-ref} will allow all openssl headers but
disallow any others.

```c++
#include <curses.h>       // Bad: disallowed system header.
#include <openssl/ssl.h>  // Good: allowed system header.
#include <openssl/rsa.h>  // Good: allowed system header.
#include <zlib.h>         // Bad: disallowed system header.
#include "src/myfile.h"   // Good: non-system header always allowed.
```

## Options

::: option
Includes

A string containing a comma separated glob list of allowed include
filenames. Similar to the -checks glob list for running clang-tidy
itself, the two wildcard characters are [\*]{.title-ref} and
[-]{.title-ref}, to include and exclude globs, respectively. The default
is [\*]{.title-ref}, which allows all includes.
:::
