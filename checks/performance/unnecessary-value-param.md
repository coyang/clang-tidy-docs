# performance-unnecessary-value-param

Flags value parameter declarations of expensive to copy types that are
copied for each invocation but it would suffice to pass them by const
reference.

The check is only applied to parameters of types that are expensive to
copy which means they are not trivially copyable or have a non-trivial
copy constructor or destructor.

To ensure that it is safe to replace the value parameter with a const
reference the following heuristic is employed:

1.  the parameter is const qualified;
2.  the parameter is not const, but only const methods or operators are
    invoked on it, or it is used as const reference or value argument in
    constructors or function calls.

Example:

```c++
void f(const string Value) {
  // The warning will suggest making Value a reference.
}

void g(ExpensiveToCopy Value) {
  // The warning will suggest making Value a const reference.
  Value.ConstMethd();
  ExpensiveToCopy Copy(Value);
}
```

If the parameter is not const, only copied or assigned once and has a
non-trivial move-constructor or move-assignment operator respectively
the check will suggest to move it.

Example:

```c++
void setValue(string Value) {
  Field = Value;
}
```

Will become:

```c++
#include <utility>

void setValue(string Value) {
  Field = std::move(Value);
}
```

Because the fix-it needs to change the signature of the function, it may
break builds if the function is used in multiple translation units or
some codes depends on function signatures.

## Options

::: option
IncludeStyle

A string specifying which include-style is used, [llvm]{.title-ref} or
[google]{.title-ref}. Default is [llvm]{.title-ref}.
:::

::: option
AllowedTypes

A semicolon-separated list of names of types allowed to be passed by
value. Regular expressions are accepted, e.g. `[Rr]ef(erence)?$` matches
every type with suffix `Ref`, `ref`, `Reference` and `reference`. The
default is empty. If a name in the list contains the sequence
[::]{.title-ref}, it is matched against the qualified type name (i.e.
`namespace::Type`), otherwise it is matched against only the type name
(i.e. `Type`).
:::

::: option
IgnoreCoroutines

A boolean specifying whether the check should suggest passing parameters
by reference in coroutines. Passing parameters by reference in
coroutines may not be safe, please see
`cppcoreguidelines-avoid-reference-coroutine-parameters <../cppcoreguidelines/avoid-reference-coroutine-parameters>`{.interpreted-text
role="doc"} for more information. Default is [true]{.title-ref}.
:::
