# bugprone-suspicious-string-compare

Find suspicious usage of runtime string comparison functions. This check
is valid in C and C++.

Checks for calls with implicit comparator and proposed to explicitly add
it.

```c++
if (strcmp(...))       // Implicitly compare to zero
if (!strcmp(...))      // Won't warn
if (strcmp(...) != 0)  // Won't warn
```

Checks that compare function results (i.e., `strcmp`) are compared to
valid constant. The resulting value is

```
<  0    when lower than,
>  0    when greater than,
== 0    when equals.
```

A common mistake is to compare the result to [1]{.title-ref} or
[-1]{.title-ref}.

```c++
if (strcmp(...) == -1)  // Incorrect usage of the returned value.
```

Additionally, the check warns if the results value is implicitly cast to
a _suspicious_ non-integer type. It\'s happening when the returned value
is used in a wrong context.

```c++
if (strcmp(...) < 0.)  // Incorrect usage of the returned value.
```

## Options

::: option
WarnOnImplicitComparison

When [true]{.title-ref}, the check will warn on implicit comparison.
[true]{.title-ref} by default.
:::

::: option
WarnOnLogicalNotComparison

When [true]{.title-ref}, the check will warn on logical not comparison.
[false]{.title-ref} by default.
:::

::: option
StringCompareLikeFunctions

A string specifying the comma-separated names of the extra string
comparison functions. Default is an empty string. The check will detect
the following string comparison functions:
[\_\_builtin_memcmp]{.title-ref}, [\_\_builtin_strcasecmp]{.title-ref},
[\_\_builtin_strcmp]{.title-ref}, [\_\_builtin_strncasecmp]{.title-ref},
[\_\_builtin_strncmp]{.title-ref}, [\_mbscmp]{.title-ref},
[\_mbscmp_l]{.title-ref}, [\_mbsicmp]{.title-ref},
[\_mbsicmp_l]{.title-ref}, [\_mbsnbcmp]{.title-ref},
[\_mbsnbcmp_l]{.title-ref}, [\_mbsnbicmp]{.title-ref},
[\_mbsnbicmp_l]{.title-ref}, [\_mbsncmp]{.title-ref},
[\_mbsncmp_l]{.title-ref}, [\_mbsnicmp]{.title-ref},
[\_mbsnicmp_l]{.title-ref}, [\_memicmp]{.title-ref},
[\_memicmp_l]{.title-ref}, [\_stricmp]{.title-ref},
[\_stricmp_l]{.title-ref}, [\_strnicmp]{.title-ref},
[\_strnicmp_l]{.title-ref}, [\_wcsicmp]{.title-ref},
[\_wcsicmp_l]{.title-ref}, [\_wcsnicmp]{.title-ref},
[\_wcsnicmp_l]{.title-ref}, [lstrcmp]{.title-ref},
[lstrcmpi]{.title-ref}, [memcmp]{.title-ref}, [memicmp]{.title-ref},
[strcasecmp]{.title-ref}, [strcmp]{.title-ref}, [strcmpi]{.title-ref},
[stricmp]{.title-ref}, [strncasecmp]{.title-ref}, [strncmp]{.title-ref},
[strnicmp]{.title-ref}, [wcscasecmp]{.title-ref}, [wcscmp]{.title-ref},
[wcsicmp]{.title-ref}, [wcsncmp]{.title-ref}, [wcsnicmp]{.title-ref},
[wmemcmp]{.title-ref}.
:::
