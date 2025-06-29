# readability-static-definition-in-anonymous-namespace

Finds static function and variable definitions in anonymous namespace.

In this case, `static` is redundant, because anonymous namespace limits
the visibility of definitions to a single translation unit.

```c++
namespace {
  static int a = 1; // Warning.
  static const int b = 1; // Warning.
  namespace inner {
    static int c = 1; // Warning.
  }
}
```

The check will apply a fix by removing the redundant `static` qualifier.
