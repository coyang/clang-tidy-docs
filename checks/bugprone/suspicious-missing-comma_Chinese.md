# bugprone-suspicious-missing-comma

String literals placed side-by-side are concatenated at translation phase 6 (after the preprocessor). This feature is used to represent long string literal on multiple lines.

字符串字面量在翻译阶段6（即预处理器之后）会被自动连接在一起。这一特性常用于将较长的字符串字面量分布在多行中表示。

For instance, the following declarations are equivalent:

例如，以下两个声明是等价的：

```c++
const char* A[] = "This is a test";
const char* B[] = "This" " is a "    "test";
```

A common mistake done by programmers is to forget a comma between two string literals in an array initializer list.

程序员常犯的一个错误是在数组初始化列表中忘记在两个字符串字面量之间添加逗号。

```c++
const char* Test[] = {
  "line 1",
  "line 2"     // Missing comma!
  "line 3",
  "line 4",
  "line 5"
};
```

The array contains the string "line 2line3" at offset 1 (i.e. Test[1]). Clang won't generate warnings at compile time.

该数组在索引1处（即 Test[1]）包含字符串 "line 2line3"。Clang 在编译时不会对此发出警告。

This check may warn incorrectly on cases like:

此检查在如下情况下可能会错误地发出警告：

```c++
const char* SupportedFormat[] = {
  "Error %s",
  "Code " PRIu64,   // May warn here.
  "Warning %s",
};
```

## Options

## 选项

::: option
SizeThreshold

An unsigned integer specifying the minimum size of a string literal to be considered by the check. Default is 5U.

一个无符号整数，指定检查中要考虑的字符串字面量的最小长度。默认值为 5U。
:::

::: option
RatioThreshold

A string specifying the maximum threshold ratio [0, 1.0] of suspicious string literals to be considered. Default is ".2".

一个字符串，指定可疑字符串字面量所占比例的最大阈值，范围为 [0, 1.0]。默认值为 ".2"。
:::

::: option
MaxConcatenatedTokens

An unsigned integer specifying the maximum number of concatenated tokens. Default is 5U.

一个无符号整数，指定允许连接的最大标记数量。默认值为 5U。
:::
