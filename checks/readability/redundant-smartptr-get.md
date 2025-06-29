# readability-redundant-smartptr-get

Find and remove redundant calls to smart pointer\'s `.get()` method.

Examples:

```c++
ptr.get()->Foo()  ==>  ptr->Foo()
*ptr.get()  ==>  *ptr
*ptr->get()  ==>  **ptr
if (ptr.get() == nullptr) ... => if (ptr == nullptr) ...
```

## Options

::: option
IgnoreMacros

If this option is set to [true]{.title-ref} (default is
[true]{.title-ref}), the check will not warn about calls inside macros.
:::
