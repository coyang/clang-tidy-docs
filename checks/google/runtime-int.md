# google-runtime-int

Finds uses of `short`, `long` and `long long` and suggest replacing them
with `u?intXX(_t)?`.

The corresponding style guide rule:
<https://google.github.io/styleguide/cppguide.html#Integer_Types>.

Corresponding cpplint.py check: [runtime/int]{.title-ref}.

## Options

::: option
UnsignedTypePrefix

A string specifying the unsigned type prefix. Default is
[uint]{.title-ref}.
:::

::: option
SignedTypePrefix

A string specifying the signed type prefix. Default is
[int]{.title-ref}.
:::

::: option
TypeSuffix

A string specifying the type suffix. Default is an empty string.
:::
