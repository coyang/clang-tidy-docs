# readability-redundant-access-specifiers

Finds classes, structs, and unions containing redundant member (field
and method) access specifiers.

## Example

```c++
class Foo {
public:
  int x;
  int y;
public:
  int z;
protected:
  int a;
public:
  int c;
}
```

In the example above, the second `public` declaration can be removed
without any changes of behavior.

## Options

::: option
CheckFirstDeclaration

If set to [true]{.title-ref}, the check will also diagnose if the first
access specifier declaration is redundant (e.g. `private` inside
`class`, or `public` inside `struct` or `union`). Default is
[false]{.title-ref}.
:::

### Example

```c++
struct Bar {
public:
  int x;
}
```

If [CheckFirstDeclaration]{.title-ref} option is enabled, a warning
about redundant access specifier will be emitted, because `public` is
the default member access for structs.
