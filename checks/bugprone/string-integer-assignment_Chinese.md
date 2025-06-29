# bugprone-string-integer-assignment

The check finds assignments of an integer to std::basic_string<CharT> (std::string, std::wstring, etc.). The source of the problem is the following assignment operator of std::basic_string<CharT>:

该检查会发现将整数赋值给 std::basic_string<CharT>（如 std::string、std::wstring 等）的情况。问题的根源在于 std::basic_string<CharT> 的以下赋值运算符：

```c++
basic_string& operator=( CharT ch );
```

Numeric types can be implicitly casted to character types.

数值类型可以被隐式转换为字符类型。

```c++
std::string s;
int x = 5965;
s = 6;
s = x;
```

Use the appropriate conversion functions or character literals.

请使用合适的转换函数或字符字面量。

```c++
std::string s;
int x = 5965;
s = '6';
s = std::to_string(x);
```

In order to suppress false positives, use an explicit cast.

为了抑制误报，请使用显式类型转换。

```c++
std::string s;
s = static_cast<char>(6);
```
