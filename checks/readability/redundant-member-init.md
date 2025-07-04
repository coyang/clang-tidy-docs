# readability-redundant-member-init

Finds member initializations that are unnecessary because the same
default constructor would be called if they were not present.

## Example

```c++
// Explicitly initializing the member s and v is unnecessary.
class Foo {
public:
  Foo() : s() {}

private:
  std::string s;
  std::vector<int> v {};
};
```

## Options

::: option
IgnoreBaseInCopyConstructors

Default is [false]{.title-ref}.

When [true]{.title-ref}, the check will ignore unnecessary base class
initializations within copy constructors, since some compilers issue
warnings/errors when base classes are not explicitly initialized in copy
constructors. For example, `gcc` with `-Wextra` or `-Werror=extra`
issues warning or error
`base class 'Bar' should be explicitly initialized in the copy constructor`
if `Bar()` were removed in the following example:
:::

```c++
// Explicitly initializing member s and base class Bar is unnecessary.
struct Foo : public Bar {
  // Remove s() below. If IgnoreBaseInCopyConstructors!=0, keep Bar().
  Foo(const Foo& foo) : Bar(), s() {}
  std::string s;
};
```
