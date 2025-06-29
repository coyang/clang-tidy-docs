# misc-static-assert

[cert-dcl03-c]{.title-ref} redirects here as an alias for this check.

[cert-dcl03-c]{.title-ref} 作为此检查项的别名重定向到此处。

Replaces assert() with static_assert() if the condition is evaluable at compile time.

如果条件可以在编译时求值，则将 assert() 替换为 static_assert()。

The condition of static_assert() is evaluated at compile time which is safer and more efficient.

static_assert() 的条件在编译时进行求值，这样更安全也更高效。
