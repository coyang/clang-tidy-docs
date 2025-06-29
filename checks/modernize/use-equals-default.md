# modernize-use-equals-default

This check replaces default bodies of special member functions with
`= default;`. The explicitly defaulted function declarations enable more
opportunities in optimization, because the compiler might treat
explicitly defaulted functions as trivial.

```c++
struct A {
  A() {}
  ~A();
};
A::~A() {}

// becomes

struct A {
  A() = default;
  ~A();
};
A::~A() = default;
```

::: note

Move-constructor and move-assignment operator are not supported yet.
:::

## Options

::: option
IgnoreMacros

If set to [true]{.title-ref}, the check will not give warnings inside
macros and will ignore special members with bodies contain macros or
preprocessor directives. Default is [true]{.title-ref}.
:::
