# bugprone-signed-char-misuse

# bugprone-signed-char-misuse（易出错的 signed char 误用）

[cert-str34-c]{.title-ref} redirects here as an alias for this check.  
[cert-str34-c]{.title-ref} 是此检查项的别名。

For the CERT alias, the [DiagnoseSignedUnsignedCharComparisons]{.title-ref} option is set to [false]{.title-ref}.  
对于 CERT 别名，选项 [DiagnoseSignedUnsignedCharComparisons]{.title-ref} 被设置为 [false]{.title-ref}。

Finds those signed char -> integer conversions which might indicate a programming error. The basic problem with the signed char, that it might store the non-ASCII characters as negative values. This behavior can cause a misunderstanding of the written code both when an explicit and when an implicit conversion happens.  
该检查项会查找将 signed char 转换为整数的操作，这些操作可能表明存在编程错误。signed char 的基本问题在于它可能将非 ASCII 字符存储为负值。这种行为在发生显式或隐式转换时都可能导致对代码的误解。

When the code contains an explicit signed char -> integer conversion, the human programmer probably expects that the converted value matches with the character code (a value from [0..255]), however, the actual value is in [-128..127] interval. To avoid this kind of misinterpretation, the desired way of converting from a signed char to an integer value is converting to unsigned char first, which stores all the characters in the positive [0..255] interval which matches the known character codes.  
当代码中包含显式的 signed char 到整数的转换时，程序员可能期望转换后的值与字符编码相匹配（即 [0..255] 范围内的值），但实际值却处于 [-128..127] 范围内。为了避免这种误解，推荐的做法是先将 signed char 转换为 unsigned char，这样可以将所有字符存储在 [0..255] 的正数范围内，与已知的字符编码相匹配。

In case of implicit conversion, the programmer might not actually be aware that a conversion happened and char value is used as an integer. There are some use cases when this unawareness might lead to a functionally imperfect code. For example, checking the equality of a signed char and an unsigned char variable is something we should avoid in C++ code. During this comparison, the two variables are converted to integers which have different value ranges. For signed char, the non-ASCII characters are stored as a value in [-128..-1] interval, while the same characters are stored in the [128..255] interval for an unsigned char.  
在隐式转换的情况下，程序员可能并未意识到发生了转换，而 char 值已被当作整数使用。在某些用例中，这种未察觉的转换可能导致功能不完善的代码。例如，在 C++ 代码中应避免比较 signed char 和 unsigned char 变量的相等性。在这种比较中，两个变量会被转换为整数，但它们的取值范围不同。对于 signed char，非 ASCII 字符被存储为 [-128..-1] 范围内的值，而对于 unsigned char，相同的字符则被存储为 [128..255] 范围内的值。

It depends on the actual platform whether plain char is handled as signed char by default and so it is caught by this check or not. To change the default behavior you can use -funsigned-char and -fsigned-char compilation options.  
plain char 是否默认被当作 signed char 处理取决于具体平台，因此是否被此检查项捕获也会有所不同。要更改默认行为，可以使用 -funsigned-char 和 -fsigned-char 编译选项。

Currently, this check warns in the following cases:

- signed char is assigned to an integer variable
- 将 signed char 赋值给整数变量

- signed char and unsigned char are compared with equality/inequality operator
- 使用相等/不等运算符比较 signed char 和 unsigned char

- signed char is converted to an integer in the array subscript
- 在数组下标中将 signed char 转换为整数

See also: STR34-C. Cast characters to unsigned char before converting to larger integer sizes  
另请参阅：STR34-C. 在转换为更大整数类型之前先将字符转换为 unsigned char  
https://wiki.sei.cmu.edu/confluence/display/c/STR34-C.+Cast+characters+to+unsigned+char+before+converting+to+larger+integer+sizes

A good example from the CERT description when a char variable is used to read from a file that might contain non-ASCII characters. The problem comes up when the code uses the -1 integer value as EOF, while the 255 character code is also stored as -1 in two's complement form of char type. See a simple example of this below. This code stops not only when it reaches the end of the file, but also when it gets a character with the 255 code.  
以下是来自 CERT 描述的一个很好的示例：当使用 char 变量从可能包含非 ASCII 字符的文件中读取数据时，问题就会出现。如果代码使用整数值 -1 表示 EOF（文件结束），而字符编码 255 在 char 类型的二进制补码中也被存储为 -1，就会导致问题。请参见下面的简单示例。该代码不仅在到达文件末尾时停止，还会在读取到编码为 255 的字符时停止。

```c++
#define EOF (-1)

int read(void) {
  char CChar;
  int IChar = EOF;

  if (readChar(CChar)) {
    IChar = CChar;
  }
  return IChar;
}
```

A proper way to fix the code above is converting the char variable to an unsigned char value first.  
修复上述代码的正确方法是先将 char 变量转换为 unsigned char 值。

```c++
#define EOF (-1)

int read(void) {
  char CChar;
  int IChar = EOF;

  if (readChar(CChar)) {
    IChar = static_cast<unsigned char>(CChar);
  }
  return IChar;
}
```

Another use case is checking the equality of two char variables with different signedness. Inside the non-ASCII value range this comparison between a signed char and an unsigned char always returns false.  
另一个用例是比较两个具有不同符号类型的 char 变量是否相等。在非 ASCII 值范围内，signed char 和 unsigned char 之间的这种比较总是返回 false。

```c++
bool compare(signed char SChar, unsigned char USChar) {
  if (SChar == USChar)
    return true;
  return false;
}
```

The easiest way to fix this kind of comparison is casting one of the arguments, so both arguments will have the same type.  
修复这种比较的最简单方法是对其中一个参数进行类型转换，使两个参数具有相同的类型。

```c++
bool compare(signed char SChar, unsigned char USChar) {
  if (static_cast<unsigned char>(SChar) == USChar)
    return true;
  return false;
}
```

## Options

## 选项

::: option
CharTypdefsToIgnore

A semicolon-separated list of typedef names. In this list, we can list typedefs for char or signed char, which will be ignored by the check. This is useful when a typedef introduces an integer alias like sal_Int8 or int8_t. In this case, human misinterpretation is not an issue. Default is an empty string.  
一个以分号分隔的 typedef 名称列表。在此列表中，我们可以列出 char 或 signed char 的 typedef，这些类型将被此检查项忽略。当 typedef 引入了如 sal_Int8 或 int8_t 这样的整数别名时，这非常有用。在这种情况下，不存在人为误解的问题。默认值为空字符串。
:::

::: option
DiagnoseSignedUnsignedCharComparisons

When true, the check will warn on signed char/unsigned char comparisons, otherwise these comparisons are ignored. By default, this option is set to true.  
当设置为 true 时，此检查项将在 signed char 与 unsigned char 比较时发出警告；否则将忽略这些比较。默认情况下，该选项设置为 true。
:::
