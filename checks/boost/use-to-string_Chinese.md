# boost-use-to-string

This check finds conversion from integer type like int to std::string or std::wstring using boost::lexical_cast, and replace it with calls to std::to_string and std::to_wstring.

此检查会查找使用 boost::lexical_cast 将整数类型（如 int）转换为 std::string 或 std::wstring 的情况，并将其替换为调用 std::to_string 和 std::to_wstring。

It doesn't replace conversion from floating points despite the to_string overloads, because it would change the behavior.

尽管 std::to_string 支持浮点数的重载，但本检查不会替换浮点数的转换，因为这可能会改变程序行为。

```c++
auto str = boost::lexical_cast<std::string>(42);
auto wstr = boost::lexical_cast<std::wstring>(2137LL);

// Will be changed to
// 将被替换为
auto str = std::to_string(42);
auto wstr = std::to_wstring(2137LL);
```
