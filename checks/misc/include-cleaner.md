# misc-include-cleaner

Checks for unused and missing includes. Generates findings only for the
main file of a translation unit. Findings correspond to
<https://clangd.llvm.org/design/include-cleaner>.

Example:

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

::: option
IgnoreHeaders

A semicolon-separated list of regexes to disable insertion/removal of
header files that match this regex as a suffix. E.g.,
[foo/.\*]{.title-ref} disables insertion/removal for all headers under
the directory [foo]{.title-ref}. Default is an empty string, no headers
will be ignored.
:::

::: option
DeduplicateFindings

A boolean that controls whether the check should deduplicate findings
for the same symbol. Defaults to [true]{.title-ref}.
:::

::: option
UnusedIncludes

A boolean that controls whether the check should report unused includes
(includes that are not used directly). Defaults to [true]{.title-ref}.
:::

::: option
MissingIncludes

A boolean that controls whether the check should report missing includes
(header files from which symbols are used but which are not directly
included). Defaults to [true]{.title-ref}.
:::
