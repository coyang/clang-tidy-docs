# readability-braces-around-statements

[google-readability-braces-around-statements]{.title-ref} redirects here as an alias for this check.

[google-readability-braces-around-statements]{.title-ref} 是此检查项的别名，重定向至此。

Checks that bodies of if statements and loops (for, do while, and while) are inside braces.

检查 if 语句和循环（for、do while 和 while）的主体是否包含在大括号中。

Before:

之前：

```c++
if (condition)
  statement;
```

After:

之后：

```c++
if (condition) {
  statement;
}
```

## Options

## 选项

::: option
ShortStatementLines

ShortStatementLines

Defines the minimal number of lines that the statement should have in order to trigger this check.

定义触发此检查所需的语句最小行数。

The number of lines is counted from the end of condition or initial keyword (do/else) until the last line of the inner statement. Default value [0]{.title-ref} means that braces will be added to all statements (not having them already).

行数从条件或初始关键字（do/else）结束处开始计数，直到内部语句的最后一行。默认值 [0]{.title-ref} 表示将为所有语句添加大括号（如果尚未添加的话）。
:::
