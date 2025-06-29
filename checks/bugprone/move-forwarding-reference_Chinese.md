# bugprone-move-forwarding-reference

Warns if std::move is called on a forwarding reference, for example:

如果在转发引用上调用了 std::move，则会发出警告，例如：

```c++
template <typename T>
void foo(T&& t) {
  bar(std::move(t));
}
```

Forwarding references should typically be passed to std::forward instead of std::move, and this is the fix that will be suggested.

转发引用通常应传递给 std::forward 而不是 std::move，这是建议的修复方式。

(A forwarding reference is an rvalue reference of a type that is a deduced function template argument.)

（转发引用是指类型为推导出的函数模板参数的右值引用。）

In this example, the suggested fix would be

在此示例中，建议的修复方式为：

```c++
bar(std::forward<T>(t));
```

## Background

代码如上例所示，有时是基于以下预期编写的：无论 T 被推导为何种类型，T&& 最终总是一个右值引用，因此无法将左值传递给 foo()。然而，这种预期并不正确。请看以下示例：

Code like the example above is sometimes written with the expectation that T&& will always end up being an rvalue reference, no matter what type is deduced for T, and that it is therefore not possible to pass an lvalue to foo(). However, this is not true. Consider this example:

```c++
std::string s = "Hello, world";
foo(s);
```

This code compiles and, after the call to foo(), s is left in an indeterminate state because it has been moved from. This may be surprising to the caller of foo() because no std::move was used when calling foo().

这段代码可以编译，并且在调用 foo() 之后，s 会处于未定义状态，因为它已经被移动了。由于在调用 foo() 时并未使用 std::move，这种行为可能会让 foo() 的调用者感到意外。

The reason for this behavior lies in the special rule for template argument deduction on function templates like foo() -- i.e. on function templates that take an rvalue reference argument of a type that is a deduced function template argument. (See section [temp.deduct.call]/3 in the C++11 standard.)

这种行为的原因在于像 foo() 这样的函数模板在模板参数推导时存在特殊规则 —— 即函数模板的参数是推导出的类型的右值引用。（参见 C++11 标准中的 [temp.deduct.call]/3 节。）

If foo() is called on an lvalue (as in the example above), then T is deduced to be an lvalue reference. In the example, T is deduced to be std::string &. The type of the argument t therefore becomes std::string& &&; by the reference collapsing rules, this collapses to std::string&.

如果对一个左值调用 foo()（如上例所示），那么 T 会被推导为左值引用。在该示例中，T 被推导为 std::string &。因此，参数 t 的类型变为 std::string& &&；根据引用折叠规则，这会折叠为 std::string&。

This means that the foo(s) call passes s as an lvalue reference, and foo() ends up moving s and thereby placing it into an indeterminate state.

这意味着 foo(s) 调用将 s 作为左值引用传递，而 foo() 最终对 s 执行了移动操作，从而使其处于未定义状态。
