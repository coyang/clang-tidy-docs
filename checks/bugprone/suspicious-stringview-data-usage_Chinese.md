# bugprone-suspicious-stringview-data-usage

Identifies suspicious usages of std::string_view::data() that could lead to reading out-of-bounds data due to inadequate or incorrect string null termination.

识别对 std::string_view::data() 的可疑使用，这些使用可能由于字符串未正确以空字符结尾而导致越界读取数据。

It warns when the result of data() is passed to a constructor or function without also passing the corresponding result of size() or length() member function. Such usage can lead to unintended behavior, particularly when assuming the data pointed to by data() is null-terminated.

当 data() 的结果被传递给构造函数或函数，而没有同时传递 size() 或 length() 成员函数的结果时，该检查会发出警告。这种用法可能导致意外行为，特别是在假设 data() 所指向的数据是以空字符结尾的情况下。

The absence of a c_str() method in std::string_view often leads developers to use data() as a substitute, especially when interfacing with C APIs that expect null-terminated strings. However, since data() does not guarantee null termination, this can result in unintended behavior if the API relies on proper null termination for correct string interpretation.

由于 std::string_view 中没有 c_str() 方法，开发者常常使用 data() 作为替代，特别是在与期望以空字符结尾字符串的 C API 交互时。然而，由于 data() 并不保证以空字符结尾，如果 API 依赖于正确的空字符结尾来解析字符串，这可能会导致意外行为。

In today's programming landscape, this scenario can occur when implicitly converting an std::string_view to an std::string. Since the constructor in std::string designed for string-view-like objects is explicit, attempting to pass an std::string_view to a function expecting an std::string will result in a compilation error. As a workaround, developers may be tempted to utilize the .data() method to achieve compilation, introducing potential risks.

在当今的编程环境中，当隐式地将 std::string_view 转换为 std::string 时，可能会出现这种情况。由于 std::string 中用于 string_view 类似对象的构造函数是 explicit（显式的），尝试将 std::string_view 传递给期望 std::string 的函数将导致编译错误。作为一种变通方法，开发者可能会倾向于使用 .data() 方法来通过编译，从而引入潜在风险。

For instance:

例如：

```c++
void printString(const std::string& str) {
  std::cout << "String: " << str << std::endl;
}

void something(std::string_view sv) {
  printString(sv.data());
}
```

In this example, directly passing sv to the printString function would lead to a compilation error due to the explicit nature of the std::string constructor. Consequently, developers might opt for sv.data() to resolve the compilation error, albeit introducing potential hazards as discussed.

在这个例子中，直接将 sv 传递给 printString 函数会因为 std::string 构造函数是显式的而导致编译错误。因此，开发者可能会选择使用 sv.data() 来解决编译错误，尽管这会引入前面所讨论的潜在风险。

## Options

## 选项

::: option
StringViewTypes

Option allows users to specify custom string view-like types for analysis. It accepts a semicolon-separated list of type names or regular expressions matching these types. Default value is: [::std::basic_string_view;::llvm::StringRef]{.title-ref}.

该选项允许用户指定自定义的类似 string_view 的类型以供分析。它接受由分号分隔的类型名称列表或匹配这些类型的正则表达式。默认值为：[::std::basic_string_view;::llvm::StringRef]{.title-ref}。
:::

::: option
AllowedCallees

Specifies methods, functions, or classes where the result of .data() is passed to. Allows to exclude such calls from the analysis. Accepts a semicolon-separated list of names or regular expressions matching these entities. Default value is: empty string.

指定 .data() 的结果被传递到的方法、函数或类。允许将这些调用从分析中排除。接受由分号分隔的名称或匹配这些实体的正则表达式。默认值为空字符串。
:::
