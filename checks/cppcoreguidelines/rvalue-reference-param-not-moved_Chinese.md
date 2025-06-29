# cppcoreguidelines-rvalue-reference-param-not-moved

Warns when an rvalue reference function parameter is never moved within the function body.

当一个右值引用函数参数在函数体中从未被移动（move）时，发出警告。

Rvalue reference parameters indicate a parameter that should be moved with std::move from within the function body. Any such parameter that is never moved is confusing and potentially indicative of a buggy program.

右值引用参数表示该参数应在函数体内使用 std::move 进行移动。任何此类参数如果从未被移动，可能会引起困惑，并可能表明程序存在缺陷。

Example:

示例：

```c++
void logic(std::string&& Input) {
  std::string Copy(Input); // Oops - forgot to std::move
}
```

Note that parameters that are unused and marked as such will not be diagnosed.

请注意，未使用且已标记为未使用的参数将不会被诊断。

Example:

示例：

```c++
void conditional_use([[maybe_unused]] std::string&& Input) {
  // No diagnostic here since Input is unused and marked as such
  // 此处不会发出诊断，因为 Input 未被使用且已被标记为未使用
}
```

## Options

## 选项

::: option
AllowPartialMove

If set to true, the check accepts std::move calls containing any subexpression containing the parameter. CppCoreGuideline F.18 officially mandates that the parameter itself must be moved. Default is false.

如果设置为 true，则该检查接受包含参数的任意子表达式中的 std::move 调用。CppCoreGuideline F.18 正式规定必须移动参数本身。默认值为 false。

```c++
// 'p' is flagged by this check if and only if AllowPartialMove is false
// 仅当 AllowPartialMove 为 false 时，'p' 才会被此检查标记
void move_members_of(pair<Obj, Obj>&& p) {
  pair<Obj, Obj> other;
  other.first = std::move(p.first);
  other.second = std::move(p.second);
}

// 'p' is never flagged by this check
// 'p' 永远不会被此检查标记
void move_whole_pair(pair<Obj, Obj>&& p) {
  pair<Obj, Obj> other = std::move(p);
}
```

:::

::: option
IgnoreUnnamedParams

If set to true, the check ignores unnamed rvalue reference parameters. Default is false.

如果设置为 true，则该检查会忽略未命名的右值引用参数。默认值为 false。

:::

::: option
IgnoreNonDeducedTemplateTypes

If set to true, the check ignores non-deduced template type rvalue reference parameters. Default is false.

如果设置为 true，则该检查会忽略非推导模板类型的右值引用参数。默认值为 false。

```c++
template <class T>
struct SomeClass {
  // Below, 'T' is not deduced and 'T&&' is an rvalue reference type.
  // This will be flagged if and only if IgnoreNonDeducedTemplateTypes is false.
  // One suggested fix would be to specialize the class for 'T' and 'T&' separately
  // (e.g., see std::future), or allow only one of 'T' or 'T&' instantiations of SomeClass
  // (e.g., see std::optional).
  // 以下，'T' 不是推导类型，'T&&' 是右值引用类型。
  // 仅当 IgnoreNonDeducedTemplateTypes 为 false 时，此处才会被标记。
  // 一个建议的修复方法是分别为 'T' 和 'T&' 特化该类（例如参见 std::future），
  // 或仅允许 'T' 或 'T&' 的某一种实例化（例如参见 std::optional）。
  SomeClass(T&& t) { }
};

// Never flagged, since 'T' is a forwarding reference in a deduced context
// 永远不会被标记，因为在推导上下文中 'T' 是一个转发引用
template <class T>
void forwarding_ref(T&& t) {
  T other = std::forward<T>(t);
}
```

:::

This check implements F.18 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 F.18 条款。
