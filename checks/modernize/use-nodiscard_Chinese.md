以下是完整的中英文对照翻译，保留了原始的 markdown 和代码块格式：

# modernize-use-nodiscard

Adds [[nodiscard]] attributes (introduced in C++17) to member
functions in order to highlight at compile time which return values
should not be ignored.

为成员函数添加 [[nodiscard]] 属性（在 C++17 中引入），以便在编译时突出显示哪些返回值不应被忽略。

Member functions need to satisfy the following conditions to be
considered by this check:

成员函数需满足以下条件，才会被此检查考虑：

> - no [[nodiscard]], [[noreturn]],
>   **attribute**((warn_unused_result)),
>   [[clang::warn_unused_result]] nor [[gcc::warn_unused_result]]
>   attribute,
> - non-void return type,
> - non-template return types,
> - const member function,
> - non-variadic functions,
> - no non-const reference parameters,
> - no pointer parameters,
> - no template parameters,
> - no template function parameters,
> - not be a member of a class with mutable member variables,
> - no Lambdas,
> - no conversion functions.

> - 没有 [[nodiscard]]、[[noreturn]]、**attribute**((warn_unused_result))、[[clang::warn_unused_result]] 或 [[gcc::warn_unused_result]] 属性，
> - 返回类型不是 void，
> - 返回类型不是模板类型，
> - 是 const 成员函数，
> - 不是可变参数函数，
> - 没有非常量引用参数，
> - 没有指针参数，
> - 没有模板参数，
> - 没有模板函数参数，
> - 不属于具有 mutable 成员变量的类，
> - 不是 Lambda 表达式，
> - 不是转换函数。

Such functions have no means of altering any state or passing values
other than via the return type. Unless the member functions are altering
state via some external call (e.g. I/O).

此类函数除了通过返回值外，无法更改任何状态或传递值。除非成员函数通过某些外部调用（例如 I/O）来更改状态。

## Example

示例

```c++
bool empty() const;
bool empty(int i) const;
```

transforms to:

转换为：

```c++
[[nodiscard]] bool empty() const;
[[nodiscard]] bool empty(int i) const;
```

## Options

选项

::: option
ReplacementString

Specifies a macro to use instead of [[nodiscard]]. This is useful when
maintaining source code that needs to compile with a pre-C++17 compiler.

指定一个宏来替代 [[nodiscard]]。当需要维护需要在 C++17 之前的编译器中编译的源代码时，这非常有用。
:::

### Example

示例

```c++
bool empty() const;
bool empty(int i) const;
```

transforms to:

转换为：

```c++
NO_DISCARD bool empty() const;
NO_DISCARD bool empty(int i) const;
```

if the ReplacementString option is
set to NO_DISCARD.

如果 ReplacementString 选项设置为 NO_DISCARD。

::: note

If the ReplacementString is not a C++
attribute, but instead a macro, then that macro must be defined in scope
or the fix-it will not be applied.

如果 ReplacementString 不是 C++ 属性，而是一个宏，则该宏必须在作用域中定义，否则修复操作将不会被应用。
:::

::: note

For alternative **attribute** syntax options to mark functions as
[[nodiscard]] in non-c++17 source code. See
https://clang.llvm.org/docs/AttributeReference.html#nodiscard-warn-unused-result

有关在非 C++17 源代码中使用 **attribute** 语法标记函数为 [[nodiscard]] 的替代方法，请参阅：
https://clang.llvm.org/docs/AttributeReference.html#nodiscard-warn-unused-result
:::
