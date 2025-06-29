# modernize-make-unique

This check finds the creation of `std::unique_ptr` objects by explicitly
calling the constructor and a `new` expression, and replaces it with a
call to `std::make_unique`, introduced in C++14.

```c++
auto my_ptr = std::unique_ptr<MyPair>(new MyPair(1, 2));

// becomes

auto my_ptr = std::make_unique<MyPair>(1, 2);
```

This check also finds calls to `std::unique_ptr::reset()` with a `new`
expression, and replaces it with a call to `std::make_unique`.

```c++
my_ptr.reset(new MyPair(1, 2));

// becomes

my_ptr = std::make_unique<MyPair>(1, 2);
```

## Options

::: option
MakeSmartPtrFunction

A string specifying the name of make-unique-ptr function. Default is
[std::make_unique]{.title-ref}.
:::

::: option
MakeSmartPtrFunctionHeader

A string specifying the corresponding header of make-unique-ptr
function. Default is [\<memory\>]{.title-ref}.
:::

::: option
IncludeStyle

A string specifying which include-style is used, [llvm]{.title-ref} or
[google]{.title-ref}. Default is [llvm]{.title-ref}.
:::

::: option
IgnoreMacros

If set to [true]{.title-ref}, the check will not give warnings inside
macros. Default is [true]{.title-ref}.
:::

::: option
IgnoreDefaultInitialization

If set to [false]{.title-ref}, the check does not suggest edits that
will transform default initialization into value initialization, as this
can cause performance regressions. Default is [true]{.title-ref}.
:::
