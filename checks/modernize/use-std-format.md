# modernize-use-std-format

Converts calls to `absl::StrFormat`, or other functions via
configuration options, to C++20\'s `std::format`, or another function
via a configuration option, modifying the format string appropriately
and removing now-unnecessary calls to `std::string::c_str()` and
`std::string::data()`.

For example, it turns lines like

```c++
return absl::StrFormat("The %s is %3d", description.c_str(), value);
```

into:

```c++
return std::format("The {} is {:3}", description, value);
```

The check uses the same format-string-conversion algorithm as
[modernize-use-std-print](../modernize/use-std-print.html) and its
shortcomings and behaviour in combination with macros are described in
the documentation for that check.

## Options

::: option
StrictMode

When [true]{.title-ref}, the check will add casts when converting from
variadic functions and printing signed or unsigned integer types
(including fixed-width integer types from `<cstdint>`, `ptrdiff_t`,
`size_t` and `ssize_t`) as the opposite signedness to ensure that the
output would matches that of a simple wrapper for `std::sprintf` that
accepted a C-style variable argument list. For example, with
[StrictMode]{.title-ref} enabled,

```c++
extern std::string strprintf(const char *format, ...);
int i = -42;
unsigned int u = 0xffffffff;
return strprintf("%u %d\n", i, u);
```

would be converted to

```c++
return std::format("{} {}\n", static_cast<unsigned int>(i), static_cast<int>(u));
```

to ensure that the output will continue to be the unsigned
representation of -42 and the signed representation of 0xffffffff (often
4294967254 and -1 respectively). When [false]{.title-ref} (which is the
default), these casts will not be added which may cause a change in the
output. Note that this option makes no difference for the default value
of [StrFormatLikeFunctions]{.title-ref} since `absl::StrFormat` takes a
function parameter pack and is not a variadic function.
:::

::: option
StrFormatLikeFunctions

A semicolon-separated list of (fully qualified) function names to
replace, with the requirement that the first parameter contains the
printf-style format string and the arguments to be formatted follow
immediately afterwards. Qualified member function names are supported,
but the replacement function name must be unqualified. The default value
for this option is [absl::StrFormat]{.title-ref}.
:::

::: option
ReplacementFormatFunction

The function that will be used to replace the function set by the
[StrFormatLikeFunctions]{.title-ref} option rather than the default
[std::format]{.title-ref}. It is expected that the function provides an
interface that is compatible with `std::format`. A suitable candidate
would be [fmt::format]{.title-ref}.
:::

::: option
FormatHeader

The header that must be included for the declaration of
[ReplacementFormatFunction]{.title-ref} so that a `#include` directive
can be added if required. If [ReplacementFormatFunction]{.title-ref} is
[std::format]{.title-ref} then this option will default to `<format>`,
otherwise this option will default to nothing and no `#include`
directive will be added.
:::
