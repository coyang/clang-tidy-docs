# fuchsia-default-arguments-declarations

Warns if a function or method is declared with default parameters.

For example, the declaration:

```c++
int foo(int value = 5) { return value; }
```

will cause a warning.

See the features disallowed in Fuchsia at
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en>
