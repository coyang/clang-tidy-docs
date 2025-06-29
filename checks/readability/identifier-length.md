# readability-identifier-length

This check finds variables and function parameters whose length are too
short. The desired name length is configurable.

Special cases are supported for loop counters and for exception variable
names.

## Options

The following options are described below:

> - `MinimumVariableNameLength`{.interpreted-text role="option"},
>   `IgnoredVariableNames`{.interpreted-text role="option"}
> - `MinimumParameterNameLength`{.interpreted-text role="option"},
>   `IgnoredParameterNames`{.interpreted-text role="option"}
> - `MinimumLoopCounterNameLength`{.interpreted-text role="option"},
>   `IgnoredLoopCounterNames`{.interpreted-text role="option"}
> - `MinimumExceptionNameLength`{.interpreted-text role="option"},
>   `IgnoredExceptionVariableNames`{.interpreted-text role="option"}

::: option
MinimumVariableNameLength

All variables (other than loop counter, exception names and function
parameters) are expected to have at least a length of
[MinimumVariableNameLength]{.title-ref} (default is [3]{.title-ref}).
Setting it to [0]{.title-ref} or [1]{.title-ref} disables the check
entirely.

```c++
int i = 42;    // warns that 'i' is too short
```

This check does not have any fix suggestions in the general case since
variable names have semantic value.
:::

::: option
IgnoredVariableNames

Specifies a regular expression for variable names that are to be
ignored. The default value is empty, thus no names are ignored.
:::

::: option
MinimumParameterNameLength

All function parameter names are expected to have a length of at least
[MinimumParameterNameLength]{.title-ref} (default is [3]{.title-ref}).
Setting it to [0]{.title-ref} or [1]{.title-ref} disables the check
entirely.

```c++
int doubler(int x)   // warns that x is too short
{
   return 2 * x;
}
```

This check does not have any fix suggestions in the general case since
variable names have semantic value.
:::

::: option
IgnoredParameterNames

Specifies a regular expression for parameters that are to be ignored.
The default value is [\^\[n\]\$]{.title-ref} for historical reasons.
:::

::: option
MinimumLoopCounterNameLength

Loop counter variables are expected to have a length of at least
[MinimumLoopCounterNameLength]{.title-ref} characters (default is
[2]{.title-ref}). Setting it to [0]{.title-ref} or [1]{.title-ref}
disables the check entirely.

```c++
// This warns that 'q' is too short.
for (int q = 0; q < size; ++ q) {
   // ...
}
```

:::

::: option
IgnoredLoopCounterNames

Specifies a regular expression for counter names that are to be ignored.
The default value is [\^\[ijk\_\]\$]{.title-ref}; the first three
symbols for historical reasons and the last one since it is frequently
used as a \"don\'t care\" value, specifically in tools such as Google
Benchmark.

```c++
// This does not warn by default, for historical reasons.
for (int i = 0; i < size; ++ i) {
    // ...
}
```

:::

::: option
MinimumExceptionNameLength

Exception clause variables are expected to have a length of at least
[MinimumExceptionNameLength]{.title-ref} (default is [2]{.title-ref}).
Setting it to [0]{.title-ref} or [1]{.title-ref} disables the check
entirely.

```c++
try {
    // ...
}
// This warns that 'e' is too short.
catch (const std::exception& x) {
    // ...
}
```

:::

::: option
IgnoredExceptionVariableNames

Specifies a regular expression for exception variable names that are to
be ignored. The default value is [\^\[e\]\$]{.title-ref} mainly for
historical reasons.

```c++
try {
    // ...
}
// This does not warn by default, for historical reasons.
catch (const std::exception& e) {
    // ...
}
```

:::
