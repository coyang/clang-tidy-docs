# bugprone-unintended-char-ostream-output

Finds unintended character output from unsigned char and signed char to an ostream.

查找将 unsigned char 和 signed char 输出到 ostream 时产生的非预期字符输出。

Normally, when unsigned char (uint8_t) or signed char (int8_t) is used, it is more likely a number than a character. However, when it is passed directly to std::ostream's operator<<, the result is the character output instead of the numeric value. This often contradicts the developer's intent to print integer values.

通常，当使用 unsigned char（uint8_t）或 signed char（int8_t）时，它们更可能表示数字而非字符。然而，当它们被直接传递给 std::ostream 的 operator<< 时，输出结果是字符而不是数值。这通常与开发者想要输出整数值的意图相悖。

```c++
uint8_t v = 65;
std::cout << v; // output 'A' instead of '65'
// 输出 'A' 而不是 '65'
```

The check will suggest casting the value to an appropriate type to indicate the intent, by default, it will cast to unsigned int for unsigned char and int for signed char.

该检查器会建议将值转换为合适的类型以明确意图，默认情况下，会将 unsigned char 转换为 unsigned int，将 signed char 转换为 int。

```c++
std::cout << static_cast<unsigned int>(v); // when v is unsigned char
// 当 v 是 unsigned char 时

std::cout << static_cast<int>(v); // when v is signed char
// 当 v 是 signed char 时
```

To avoid lengthy cast statements, add prefix + to the variable can also suppress warnings because unary expression will promote the value to an int.

为了避免冗长的类型转换语句，可以在变量前加上 + 前缀来抑制警告，因为一元表达式会将值提升为 int 类型。

```c++
std::cout << +v;
```

Or cast to char to explicitly indicate that output should be a character.

或者将其转换为 char 类型，以明确表示输出应为字符。

```c++
std::cout << static_cast<char>(v);
```

## Options

## 选项

::: option
AllowedTypes

A semicolon-separated list of type names that will be treated like the char type: the check will not report variables declared with with these types or explicit cast expressions to these types. Note that this distinguishes type aliases from the original type, so specifying e.g. unsigned char here will not suppress reports about uint8_t even if it is defined as a typedef alias for unsigned char. Default is [unsigned char;signed char]{.title-ref}.

一个以分号分隔的类型名称列表，这些类型将被视为 char 类型：检查器不会报告使用这些类型声明的变量或显式转换为这些类型的表达式。请注意，这里会区分类型别名和原始类型，因此即使 uint8_t 是 unsigned char 的 typedef 别名，在此处指定 unsigned char 也不会抑制关于 uint8_t 的报告。默认值为 [unsigned char;signed char]{.title-ref}。
:::

::: option
CastTypeName

When CastTypeName is specified, the fix-it will use CastTypeName as the cast target type. Otherwise, fix-it will automatically infer the type.

当指定了 CastTypeName 时，修复建议将使用 CastTypeName 作为转换目标类型。否则，修复建议将自动推断类型。
:::
