# modernize-deprecated-headers

Some headers from C library were deprecated in C++ and are no longer
welcome in C++ codebases. Some have no effect in C++. For more details
refer to the C++14 Standard \[depr.c.headers\] section.

This check replaces C standard library headers with their C++
alternatives and removes redundant ones.

```c++
// C++ source file...
#include <assert.h>
#include <stdbool.h>

// becomes

#include <cassert>
// No 'stdbool.h' here.
```

Important note: the Standard doesn\'t guarantee that the C++ headers
declare all the same functions in the global namespace. The check in its
current form can break the code that uses library symbols from the
global namespace.

- [\<assert.h\>]{.title-ref}
- [\<complex.h\>]{.title-ref}
- [\<ctype.h\>]{.title-ref}
- [\<errno.h\>]{.title-ref}
- [\<fenv.h\>]{.title-ref} // deprecated since C++11
- [\<float.h\>]{.title-ref}
- [\<inttypes.h\>]{.title-ref}
- [\<limits.h\>]{.title-ref}
- [\<locale.h\>]{.title-ref}
- [\<math.h\>]{.title-ref}
- [\<setjmp.h\>]{.title-ref}
- [\<signal.h\>]{.title-ref}
- [\<stdarg.h\>]{.title-ref}
- [\<stddef.h\>]{.title-ref}
- [\<stdint.h\>]{.title-ref}
- [\<stdio.h\>]{.title-ref}
- [\<stdlib.h\>]{.title-ref}
- [\<string.h\>]{.title-ref}
- [\<tgmath.h\>]{.title-ref} // deprecated since C++11
- [\<time.h\>]{.title-ref}
- [\<uchar.h\>]{.title-ref} // deprecated since C++11
- [\<wchar.h\>]{.title-ref}
- [\<wctype.h\>]{.title-ref}

If the specified standard is older than C++11 the check will only
replace headers deprecated before C++11, otherwise \-- every header that
appeared in the previous list.

These headers don\'t have effect in C++:

- [\<iso646.h\>]{.title-ref}
- [\<stdalign.h\>]{.title-ref}
- [\<stdbool.h\>]{.title-ref}

The checker ignores [include]{.title-ref} directives within [extern
\"C\" { \... }]{.title-ref} blocks, since a library might want to expose
some API for C and C++ libraries.

```c++
// C++ source file...
extern "C" {
#include <assert.h>  // Left intact.
#include <stdbool.h> // Left intact.
}
```

## Options

::: option
CheckHeaderFile

[clang-tidy]{.title-ref} cannot know if the header file included by the
currently analyzed C++ source file is not included by any other C source
files. Hence, to omit false-positives and wrong fixit-hints, we ignore
emitting reports into header files. One can set this option to
[true]{.title-ref} if they know that the header files in the project are
only used by C++ source files. Default is [false]{.title-ref}.
:::
