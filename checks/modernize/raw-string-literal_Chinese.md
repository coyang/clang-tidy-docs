# modernize-raw-string-literal

# modernize-raw-string-literal（现代化原始字符串字面量）

This check selectively replaces string literals containing escaped characters with raw string literals.

此检查会有选择地将包含转义字符的字符串字面量替换为原始字符串字面量。

Example:

示例：

```c++
const char *const Quotes{"embedded \"quotes\""};
const char *const Paragraph{"Line one.\nLine two.\nLine three.\n"};
const char *const SingleLine{"Single line.\n"};
const char *const TrailingSpace{"Look here -> \n"};
const char *const Tab{"One\tTwo\n"};
const char *const Bell{"Hello!\a  And welcome!"};
const char *const Path{"C:\\Program Files\\Vendor\\Application.exe"};
const char *const RegEx{"\\w\\([a-z]\\)"};
```

becomes

变为

```c++
const char *const Quotes{R"(embedded "quotes")"};
const char *const Paragraph{"Line one.\nLine two.\nLine three.\n"};
const char *const SingleLine{"Single line.\n"};
const char *const TrailingSpace{"Look here -> \n"};
const char *const Tab{"One\tTwo\n"};
const char *const Bell{"Hello!\a  And welcome!"};
const char *const Path{R"(C:\Program Files\Vendor\Application.exe)"};
const char *const RegEx{R"(\w\([a-z]\))"};
```

The presence of any of the following escapes can cause the string to be converted to a raw string literal: \\, \', \", \?, and octal or hexadecimal escapes for printable ASCII characters.

如果字符串中包含以下任意一种转义字符，则可能会被转换为原始字符串字面量：\\、\'、\"、\?，以及用于可打印 ASCII 字符的八进制或十六进制转义。

A string literal containing only escaped newlines is a common way of writing lines of text output. Introducing physical newlines with raw string literals in this case is likely to impede readability. These string literals are left unchanged.

仅包含转义换行符的字符串字面量是输出多行文本的一种常见写法。在这种情况下使用原始字符串字面量引入实际换行符可能会降低可读性。因此，这类字符串字面量将保持不变。

An escaped horizontal tab, form feed, or vertical tab prevents the string literal from being converted. The presence of a horizontal tab, form feed or vertical tab in source code is not visually obvious.

如果字符串中包含转义的水平制表符（\t）、换页符（\f）或垂直制表符（\v），则不会进行转换。因为在源代码中，这些字符的存在不容易被察觉。

## Options

## 选项

::: option
DelimiterStem

Custom delimiter to escape characters in raw string literals. It is used in the following construction: R"stem_delimiter(contents)stem_delimiter". The default value is lit.

用于在原始字符串字面量中转义字符的自定义定界符。其使用方式如下：R"stem_delimiter(contents)stem_delimiter"。默认值为 lit。

:::

::: option
ReplaceShorterLiterals

Controls replacing shorter non-raw string literals with longer raw string literals. Setting this option to true enables the replacement. The default value is false (shorter literals are not replaced).

控制是否将较短的普通字符串字面量替换为较长的原始字符串字面量。将此选项设置为 true 可启用替换。默认值为 false（不会替换较短的字面量）。

:::
