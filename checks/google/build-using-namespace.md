# google-build-using-namespace

Finds `using namespace` directives.

The check implements the following rule of the [Google C++ Style
Guide](https://google.github.io/styleguide/cppguide.html#Namespaces):

> You may not use a using-directive to make all names from a namespace
> available.

```c++
// Forbidden -- This pollutes the namespace.
using namespace foo;
```

Corresponding cpplint.py check name: [build/namespaces]{.title-ref}.
