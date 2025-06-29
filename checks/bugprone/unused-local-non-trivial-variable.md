# bugprone-unused-local-non-trivial-variable

Warns when a local non trivial variable is unused within a function. The
following types of variables are excluded from this check:

- trivial and trivially copyable
- references and pointers
- exception variables in catch clauses
- static or thread local
- structured bindings
- variables with `[[maybe_unused]]` attribute
- name-independent variables

This check can be configured to warn on all non-trivial variables by
setting [IncludeTypes]{.title-ref} to [.\*]{.title-ref}, and excluding
specific types using [ExcludeTypes]{.title-ref}.

In the this example, [my_lock]{.title-ref} would generate a warning that
it is unused.

```c++
std::mutex my_lock;
// my_lock local variable is never used
```

In the next example, [future2]{.title-ref} would generate a warning that
it is unused.

```c++
std::future<MyObject> future1;
std::future<MyObject> future2;
// ...
MyObject foo = future1.get();
// future2 is not used.
```

## Options

::: option
IncludeTypes

Semicolon-separated list of regular expressions matching types of
variables to check. By default the following types are checked:

- [::std::.\*mutex]{.title-ref}
- [::std::future]{.title-ref}
- [::std::basic_string]{.title-ref}
- [::std::basic_regex]{.title-ref}
- [::std::basic_istringstream]{.title-ref}
- [::std::basic_stringstream]{.title-ref}
- [::std::bitset]{.title-ref}
- [::std::filesystem::path]{.title-ref}
  :::

::: option
ExcludeTypes

A semicolon-separated list of regular expressions matching types that
are excluded from the [IncludeTypes]{.title-ref} matches. By default it
is an empty list.
:::
