# misc-uniqueptr-reset-release

Find and replace `unique_ptr::reset(release())` with `std::move()`.

Example:

```c++
std::unique_ptr<Foo> x, y;
x.reset(y.release()); -> x = std::move(y);
```

If `y` is already rvalue, `std::move()` is not added. `x` and `y` can
also be `std::unique_ptr<Foo>*`.

## Options

::: option
IncludeStyle

A string specifying which include-style is used, [llvm]{.title-ref} or
[google]{.title-ref}. Default is [llvm]{.title-ref}.
:::
