# readability-redundant-string-cstr

Finds unnecessary calls to std::string::c_str() and std::string::data().

查找对 std::string::c_str() 和 std::string::data() 的不必要调用。

## Options

## 选项

::: option
StringParameterFunctions

A semicolon-separated list of (fully qualified) function/method/operator names, with the requirement that any parameter currently accepting a const char\* input should also be able to accept std::string inputs, or proper overload candidates that can do so should exist. This can be used to configure functions such as fmt::format, spdlog::logger::info, or wrappers around these and similar functions. The default value is the empty string.

一个由分号分隔的（完全限定的）函数/方法/运算符名称列表，要求这些函数的任何接受 const char\* 输入的参数也应能够接受 std::string 输入，或者应存在可以接受 std::string 的合适重载版本。此选项可用于配置如 fmt::format、spdlog::logger::info 或这些函数及类似函数的封装器。默认值为空字符串。
:::
