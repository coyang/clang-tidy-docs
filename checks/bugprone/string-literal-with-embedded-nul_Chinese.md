# bugprone-string-literal-with-embedded-nul

Finds occurrences of string literal with embedded NUL character and validates their usage.

查找包含嵌入式 NUL 字符的字符串字面量，并验证其用法。

## Invalid escaping

Special characters can be escaped within a string literal by using their hexadecimal encoding like \x42. A common mistake is to escape them like this \0x42 where the \0 stands for the NUL character.

可以使用十六进制编码（如 \x42）在字符串字面量中转义特殊字符。一个常见的错误是将其写成 \0x42，其中 \0 表示 NUL 字符。

```c++
const char* Example[] = "Invalid character: \0x12 should be \x12";
const char* Bytes[] = "\x03\0x02\0x01\0x00\0xFF\0xFF\0xFF";
```

```c++
const char* Example[] = "无效字符：\0x12 应该写成 \x12";
const char* Bytes[] = "\x03\0x02\0x01\0x00\0xFF\0xFF\0xFF";
```

## Truncated literal

String-like classes can manipulate strings with embedded NUL as they are keeping track of the bytes and the length. This is not the case for a char\* (NUL-terminated) string.

字符串类可以处理包含嵌入式 NUL 的字符串，因为它们会记录字节数和长度。而 char\*（以 NUL 结尾的字符串）则不具备这种能力。

A common mistake is to pass a string-literal with embedded NUL to a string constructor expecting a NUL-terminated string. The bytes after the first NUL character are truncated.

一个常见的错误是将包含嵌入式 NUL 的字符串字面量传递给期望 NUL 结尾字符串的构造函数。第一个 NUL 字符之后的字节将被截断。

```c++
std::string str("abc\0def");  // "def" is truncated
str += "\0";                  // This statement is doing nothing
if (str == "\0abc") return;   // This expression is always true
```

```c++
std::string str("abc\0def");  // "def" 被截断
str += "\0";                  // 此语句无任何作用
if (str == "\0abc") return;   // 此表达式始终为真
```
