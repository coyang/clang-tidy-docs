# modernize-min-max-use-initializer-list

Replaces nested `std::min` and `std::max` calls with an initializer list
where applicable.

For instance, consider the following code:

```cpp
int a = std::max(std::max(i, j), k);
```

The check will transform the above code to:

```cpp
int a = std::max({i, j, k});
```

# Performance Considerations

While this check simplifies the code and makes it more readable, it may
cause performance degradation for non-trivial types due to the need to
copy objects into the initializer list.

To avoid this, it is recommended to use [std::ref]{.title-ref} or
[std::cref]{.title-ref} for non-trivial types:

```cpp
std::string b = std::max({std::ref(i), std::ref(j), std::ref(k)});
```

# Options

::: option
IncludeStyle

A string specifying which include-style is used, [llvm]{.title-ref} or
[google]{.title-ref}. Default is [llvm]{.title-ref}.
:::

::: option
IgnoreNonTrivialTypes

A boolean specifying whether to ignore non-trivial types. Default is
[true]{.title-ref}.
:::

::: option
IgnoreTrivialTypesOfSizeAbove

An integer specifying the size (in bytes) above which trivial types are
ignored. Default is [32]{.title-ref}.
:::
