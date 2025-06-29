# modernize-use-nullptr

The check converts the usage of null pointer constants (e.g. `NULL`,
`0`) to use the new C++11 and C23 `nullptr` keyword.

## Example

```c++
void assignment() {
  char *a = NULL;
  char *b = 0;
  char c = 0;
}

int *ret_ptr() {
  return 0;
}
```

transforms to:

```c++
void assignment() {
  char *a = nullptr;
  char *b = nullptr;
  char c = 0;
}

int *ret_ptr() {
  return nullptr;
}
```

## Options

::: option
IgnoredTypes

Semicolon-separated list of regular expressions to match pointer types
for which implicit casts will be ignored. Default value:
[std::\_CmpUnspecifiedParam::;\^std::\_\_cmp_cat::\_\_unspec]{.title-ref}.
:::

::: option
NullMacros

Comma-separated list of macro names that will be transformed along with
`NULL`. By default this check will only replace the `NULL` macro and
will skip any similar user-defined macros.
:::

### Example

```c++
#define MY_NULL (void*)0
void assignment() {
  void *p = MY_NULL;
}
```

transforms to:

```c++
#define MY_NULL NULL
void assignment() {
  int *p = nullptr;
}
```

if the `NullMacros`{.interpreted-text role="option"} option is set to
`MY_NULL`.
