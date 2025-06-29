# readability-qualified-auto

Adds pointer qualifications to auto-typed variables that are deduced to pointers.

为推导为指针的 auto 类型变量添加指针限定符。

[LLVM Coding Standards](https://llvm.org/docs/CodingStandards.html#beware-unnecessary-copies-with-auto) advises to make it obvious if a auto typed variable is a pointer. This check will transform auto to auto \* when the type is deduced to be a pointer.

[LLVM 编码规范](https://llvm.org/docs/CodingStandards.html#beware-unnecessary-copies-with-auto) 建议在 auto 类型变量为指针时应明确标示。此检查会在类型被推导为指针时将 auto 转换为 auto \*。

```c++
for (auto Data : MutatablePtrContainer) {
  change(*Data);
}
for (auto Data : ConstantPtrContainer) {
  observe(*Data);
}
```

Would be transformed into:

将被转换为：

```c++
for (auto *Data : MutatablePtrContainer) {
  change(*Data);
}
for (const auto *Data : ConstantPtrContainer) {
  observe(*Data);
}
```

Note const volatile qualified types will retain their const and volatile qualifiers. Pointers to pointers will not be fully qualified.

注意，带有 const 和 volatile 限定符的类型将保留其 const 和 volatile 限定符。对于指向指针的指针，不会完全限定所有层级。

```c++
const auto Foo = cast<int *>(Baz1);
const auto Bar = cast<const int *>(Baz2);
volatile auto FooBar = cast<int *>(Baz3);
auto BarFoo = cast<int **>(Baz4);
```

Would be transformed into:

将被转换为：

```c++
auto *const Foo = cast<int *>(Baz1);
const auto *const Bar = cast<const int *>(Baz2);
auto *volatile FooBar = cast<int *>(Baz3);
auto *BarFoo = cast<int **>(Baz4);
```

## Options

## 选项

::: option
AddConstToQualified

When set to true the check will add const qualifiers variables defined as auto \* or auto & when applicable. Default value is true.

当设置为 true 时，此检查将在适用的情况下为定义为 auto \* 或 auto & 的变量添加 const 限定符。默认值为 true。
:::

```c++
auto Foo1 = cast<const int *>(Bar1);
auto *Foo2 = cast<const int *>(Bar2);
auto &Foo3 = cast<const int &>(Bar3);
```

If AddConstToQualified is set to false, it will be transformed into:

如果 AddConstToQualified 设置为 false，将被转换为：

```c++
const auto *Foo1 = cast<const int *>(Bar1);
auto *Foo2 = cast<const int *>(Bar2);
auto &Foo3 = cast<const int &>(Bar3);
```

Otherwise it will be transformed into:

否则将被转换为：

```c++
const auto *Foo1 = cast<const int *>(Bar1);
const auto *Foo2 = cast<const int *>(Bar2);
const auto &Foo3 = cast<const int &>(Bar3);
```

Note in the LLVM alias, the default value is false.

注意，在 LLVM 别名中，默认值为 false。

::: option
AllowedTypes

A semicolon-separated list of names of types to ignore when auto is deduced to that type or a pointer to that type. Note that this distinguishes type aliases from the original type, so specifying e.g. my*int will not suppress reports about int even if it is defined as a typedef alias for int. Regular expressions are accepted, e.g. [Rr]ef(erence)?$ matches every type with suffix Ref, ref, Reference and reference. If a name in the list contains the sequence :: it is matched against the qualified type name (i.e. namespace::Type), otherwise it is matched against only the type name (i.e. Type). E.g. to suppress reports for std::array iterators use std::array<.\*>::(const*)?iterator string. The default is an empty string.

一个以分号分隔的类型名称列表，当 auto 被推导为该类型或指向该类型的指针时将忽略这些类型。注意，该选项区分类型别名和原始类型，因此即使 my*int 是 int 的 typedef 别名，指定 my_int 也不会抑制对 int 的报告。支持正则表达式，例如 [Rr]ef(erence)?$ 匹配所有以 Ref、ref、Reference 和 reference 结尾的类型。如果列表中的名称包含 :: 序列，则会与限定类型名（例如 namespace::Type）进行匹配；否则仅与类型名（例如 Type）匹配。例如，要抑制对 std::array 迭代器的报告，可使用 std::array<.\*>::(const*)?iterator 字符串。默认值为空字符串。
:::
