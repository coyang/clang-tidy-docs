# misc-static-assert

[cert-dcl03-c]{.title-ref} redirects here as an alias for this check.

Replaces `assert()` with `static_assert()` if the condition is evaluable
at compile time.

The condition of `static_assert()` is evaluated at compile time which is
safer and more efficient.
