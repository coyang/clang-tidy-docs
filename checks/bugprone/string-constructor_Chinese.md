# bugprone-string-constructor

Finds string constructors that are suspicious and probably errors.

查找可疑且可能存在错误的字符串构造函数。

A common mistake is to swap parameters to the 'fill' string-constructor.

一个常见的错误是将“填充”字符串构造函数的参数顺序写反。

Examples:

示例：

```c++
std::string str('x', 50); // should be str(50, 'x')
                          // 应该是 str(50, 'x')
```

Calling the string-literal constructor with a length bigger than the literal is suspicious and adds extra random characters to the string.

调用字符串字面值构造函数时，如果指定的长度大于实际字面值长度，则可能会导致字符串中包含额外的随机字符，这是一个可疑的行为。

Examples:

示例：

```c++
std::string("test", 200);   // Will include random characters after "test".
                            // "test" 后面会包含随机字符。
std::string("test", 2, 5);  // Will include random characters after "st".
                            // "st" 后面会包含随机字符。
std::string_view("test", 200);
                            // "test" 后面会包含随机字符。
```

Creating an empty string from constructors with parameters is considered suspicious. The programmer should use the empty constructor instead.

使用带参数的构造函数来创建空字符串被认为是可疑的。程序员应当使用默认的空构造函数。

Examples:

示例：

```c++
std::string("test", 0);   // Creation of an empty string.
                          // 创建了一个空字符串。
std::string("test", 1, 0);
                          // 创建了一个空字符串。
std::string_view("test", 0);
                          // 创建了一个空字符串。
```

Passing an invalid first character position parameter to constructor will cause std::out_of_range exception at runtime.

向构造函数传递无效的首字符位置参数将在运行时导致 std::out_of_range 异常。

Examples:

示例：

```c++
std::string("test", -1, 10); // Negative first character position.
                             // 首字符位置为负数。
std::string("test", 10, 10); // First character position is bigger than string literal character range".
                             // 首字符位置超出了字符串字面值的范围。
```

## Options

## 选项

::: option
WarnOnLargeLength

When true, the check will warn on a string with a length greater than LargeLengthThreshold.  
Default is true.

当设置为 true 时，检查器会对长度大于 LargeLengthThreshold 的字符串发出警告。  
默认值为 true。
:::

::: option
LargeLengthThreshold

An integer specifying the large length threshold.  
Default is 0x800000.

一个整数，用于指定“大长度”的阈值。  
默认值为 0x800000。
:::

::: option
StringNames

Default is ::std::basic_string;::std::basic_string_view.

Semicolon-delimited list of class names to apply this check to.  
By default ::std::basic_string applies to std::string and std::wstring.  
Set to e.g. ::std::basic_string;llvm::StringRef;QString to perform this check on custom classes.

默认值为 ::std::basic_string;::std::basic_string_view。

以分号分隔的类名列表，指定要应用此检查的类。  
默认情况下，::std::basic_string 会应用于 std::string 和 std::wstring。  
例如设置为 ::std::basic_string;llvm::StringRef;QString 可对自定义类执行此检查。
:::
