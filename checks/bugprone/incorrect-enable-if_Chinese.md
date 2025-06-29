# bugprone-incorrect-enable-if

Detects incorrect usages of std::enable_if that don't name the nested type type.

检测未命名嵌套类型 type 的 std::enable_if 错误用法。

In C++11 introduced std::enable_if as a convenient way to leverage SFINAE. One form of using std::enable_if is to declare an unnamed template type parameter with a default type equal to typename std::enable_if<condition>::type. If the author forgets to name the nested type type, then the code will always consider the candidate template even if the condition is not met.

C++11 引入了 std::enable_if，作为一种便捷的方式来利用 SFINAE。其中一种使用 std::enable_if 的方式是声明一个未命名的模板类型参数，并将其默认类型设为 typename std::enable_if<condition>::type。如果作者忘记命名嵌套类型 type，那么即使条件不满足，该模板也始终会被视为候选，从而导致错误。

Below are some examples of code using std::enable_if correctly and incorrect examples that this check flags.

以下是一些正确使用 std::enable_if 的代码示例，以及此检查器会标记的错误示例。

```c++
template <typename T, typename = typename std::enable_if<T::some_trait>::type>
void valid_usage() { ... }

template <typename T, typename = std::enable_if_t<T::some_trait>>
void valid_usage_with_trait_helpers() { ... }

// The below code is not a correct application of SFINAE. Even if
// T::some_trait is not true, the function will still be considered in the
// set of function candidates. It can either incorrectly select the function
// when it should not be a candidates, and/or lead to hard compile errors
// if the body of the template does not compile if the condition is not
// satisfied.
template <typename T, typename = std::enable_if<T::some_trait>>
void invalid_usage() { ... }

// The tool suggests the following replacement for 'invalid_usage':
template <typename T, typename = typename std::enable_if<T::some_trait>::type>
void fixed_invalid_usage() { ... }
```

```c++
// 以下代码不是对 SFINAE 的正确应用。即使 T::some_trait 不为 true，
// 该函数仍会被视为候选函数之一。这可能导致函数在不应被选中的情况下被错误地选中，
// 并/或在条件不满足时模板主体无法编译，从而引发严重的编译错误。
template <typename T, typename = std::enable_if<T::some_trait>>
void invalid_usage() { ... }

// 工具建议将 'invalid_usage' 替换为以下形式：
template <typename T, typename = typename std::enable_if<T::some_trait>::type>
void fixed_invalid_usage() { ... }
```

C++14 introduced the trait helper std::enable_if_t which reduces the likelihood of this error. C++20 introduces constraints, which generally supersede the use of std::enable_if. See modernize-type-traits for another tool that will replace std::enable_if with std::enable_if_t, and see modernize-use-constraints for another tool that replaces std::enable_if with C++20 constraints. Consider these newer mechanisms where possible.

C++14 引入了 trait 助手 std::enable_if_t，从而降低了此类错误发生的可能性。C++20 引入了约束（constraints），通常可以取代 std::enable_if 的使用。请参阅 modernize-type-traits 工具，它可将 std::enable_if 替换为 std::enable_if_t；另请参阅 modernize-use-constraints 工具，它可将 std::enable_if 替换为 C++20 的约束。建议在可能的情况下使用这些更新的机制。
