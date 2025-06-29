以下是完整的中英文双语翻译内容，保留了原始 markdown 和代码块格式，中文翻译位于英文下方，并以一个空行分隔：

# modernize-use-constraints

Replace std::enable_if with C++20 requires clauses.

使用 C++20 的 requires 子句替换 std::enable_if。

std::enable_if is a SFINAE mechanism for selecting the desired
function or class template based on type traits or other requirements.
enable_if changes the meta-arity of the template, and has other
adverse side effects in the code. C++20 introduces concepts and constraints as a cleaner
language provided solution to achieve the same outcome.

std::enable_if 是一种 SFINAE（替代失败不是错误）机制，用于根据类型特征或其他要求选择所需的函数或类模板。enable_if 会改变模板的元参数数量，并在代码中引入其他不良副作用。C++20 引入了 concepts 和 constraints，作为一种更简洁的语言级解决方案，以实现相同的目的。

This check finds some common std::enable_if patterns that can be
replaced by C++20 requires clauses. The tool can replace some of these
patterns automatically, otherwise, the tool will emit a diagnostic
without a replacement. The tool can detect the following
std::enable_if patterns

此检查器会查找一些常见的 std::enable_if 使用模式，这些模式可以被 C++20 的 requires 子句所替代。该工具可以自动替换其中的一些模式，对于无法自动替换的情况，工具将发出诊断信息但不会进行替换。该工具可以检测以下 std::enable_if 模式：

1.  std::enable_if in the return type of a function  
    函数返回类型中的 std::enable_if

2.  std::enable_if as the trailing template parameter for function templates  
    函数模板中作为尾部模板参数的 std::enable_if

Other uses, for example, in class templates for function parameters, are
not currently supported by this tool. Other variants such as
boost::enable_if are not currently supported by this tool.

其他用法，例如在类模板中用于函数参数的情况，目前该工具尚不支持。其他变体，如 boost::enable_if，目前也不受支持。

Below are some examples of code using std::enable_if.

以下是一些使用 std::enable_if 的代码示例。

```c++
// enable_if in function return type
template <typename T>
std::enable_if_t<T::some_trait, int> only_if_t_has_the_trait() { ... }

// 函数返回类型中的 enable_if
template <typename T>
std::enable_if_t<T::some_trait, int> only_if_t_has_the_trait() { ... }

// enable_if in the trailing template parameter
template <typename T, std::enable_if_t<T::some_trait, int> = 0>
void another_version() { ... }

// 作为尾部模板参数的 enable_if
template <typename T, std::enable_if_t<T::some_trait, int> = 0>
void another_version() { ... }

template <typename T>
typename std::enable_if<T::some_value, Obj>::type existing_constraint() requires (T::another_value) {
  return Obj{};
}

// 同时使用 enable_if 和 requires 的示例
template <typename T>
typename std::enable_if<T::some_value, Obj>::type existing_constraint() requires (T::another_value) {
  return Obj{};
}

template <typename T, std::enable_if_t<T::some_trait, int> = 0>
struct my_class {};

// 在类模板中使用 enable_if 的示例
template <typename T, std::enable_if_t<T::some_trait, int> = 0>
struct my_class {};
```

The tool will replace the above code with,

该工具将把上述代码替换为：

```c++
// warning: use C++20 requires constraints instead of enable_if [modernize-use-constraints]
template <typename T>
int only_if_t_has_the_trait() requires T::some_trait { ... }

// 警告：使用 C++20 requires 约束替代 enable_if [modernize-use-constraints]
template <typename T>
int only_if_t_has_the_trait() requires T::some_trait { ... }

// warning: use C++20 requires constraints instead of enable_if [modernize-use-constraints]
template <typename T>
void another_version() requires T::some_trait { ... }

// 警告：使用 C++20 requires 约束替代 enable_if [modernize-use-constraints]
template <typename T>
void another_version() requires T::some_trait { ... }

// The tool will emit a diagnostic for the following, but will
// not attempt to replace the code.
// warning: use C++20 requires constraints instead of enable_if [modernize-use-constraints]
template <typename T>
typename std::enable_if<T::some_value, Obj>::type existing_constraint() requires (T::another_value) {
  return Obj{};
}

// 工具会对以下代码发出诊断，但不会尝试进行替换。
// 警告：使用 C++20 requires 约束替代 enable_if [modernize-use-constraints]
template <typename T>
typename std::enable_if<T::some_value, Obj>::type existing_constraint() requires (T::another_value) {
  return Obj{};
}

// The tool will not emit a diagnostic or attempt to replace the code.
template <typename T, std::enable_if_t<T::some_trait, int> = 0>
struct my_class {};

// 工具不会对以下代码发出诊断，也不会尝试进行替换。
template <typename T, std::enable_if_t<T::some_trait, int> = 0>
struct my_class {};
```

::: note

System headers are not analyzed by this check.

系统头文件不会被此检查器分析。

:::
