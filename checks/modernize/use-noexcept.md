# modernize-use-noexcept

This check replaces deprecated dynamic exception specifications with the
appropriate noexcept specification (introduced in C++11). By default
this check will replace `throw()` with `noexcept`, and
`throw(<exception>[,...])` or `throw(...)` with `noexcept(false)`.

## Example

```c++
void foo() throw();
void bar() throw(int) {}
```

transforms to:

```c++
void foo() noexcept;
void bar() noexcept(false) {}
```

## Options

::: option
ReplacementString

Users can use `ReplacementString`{.interpreted-text role="option"} to
specify a macro to use instead of `noexcept`. This is useful when
maintaining source code that uses custom exception specification marking
other than `noexcept`. Fix-it hints will only be generated for
non-throwing specifications.
:::

### Example

```c++
void bar() throw(int);
void foo() throw();
```

transforms to:

```c++
void bar() throw(int);  // No fix-it generated.
void foo() NOEXCEPT;
```

if the `ReplacementString`{.interpreted-text role="option"} option is
set to [NOEXCEPT]{.title-ref}.

::: option
UseNoexceptFalse
:::

Enabled by default, disabling will generate fix-it hints that remove
throwing dynamic exception specs, e.g., `throw(<something>)`, completely
without providing a replacement text, except for destructors and delete
operators that are `noexcept(true)` by default.

### Example

```c++
void foo() throw(int) {}

struct bar {
  void foobar() throw(int);
  void operator delete(void *ptr) throw(int);
  void operator delete[](void *ptr) throw(int);
  ~bar() throw(int);
}
```

transforms to:

```c++
void foo() {}

struct bar {
  void foobar();
  void operator delete(void *ptr) noexcept(false);
  void operator delete[](void *ptr) noexcept(false);
  ~bar() noexcept(false);
}
```

if the `UseNoexceptFalse`{.interpreted-text role="option"} option is set
to [false]{.title-ref}.
