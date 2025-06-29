# readability-misleading-indentation

# 可读性 - 误导性缩进

Correct indentation helps to understand code. Mismatch of the
syntactical structure and the indentation of the code may hide serious
problems. Missing braces can also make it significantly harder to read
the code, therefore it is important to use braces.

正确的缩进有助于理解代码。语法结构与代码缩进不匹配可能会隐藏严重的问题。缺少大括号也会显著降低代码的可读性，因此使用大括号非常重要。

The way to avoid dangling else is to always check that an else belongs
to the if that begins in the same column.

避免悬空的 else 的方法是始终确保 else 对应的是与其在同一列开始的 if 语句。

You can omit braces when your inner part of e.g. an if statement has
only one statement in it. Although in that case you should begin the
next statement in the same column with the if.

当 if 语句等的内部仅包含一条语句时，可以省略大括号。但在这种情况下，下一条语句应与 if 语句在同一列开始。

Examples:

示例：

```c++
// Dangling else:
// 悬空的 else：
if (cond1)
  if (cond2)
    foo1();
else
  foo2();  // Wrong indentation: else belongs to if(cond2) statement.
// 错误的缩进：else 实际上属于 if(cond2) 语句。

// Missing braces:
// 缺少大括号：
if (cond1)
  foo1();
  foo2();  // Not guarded by if(cond1).
// 不受 if(cond1) 控制。
```

## Limitations

## 限制

Note that this check only works as expected when the tabs or spaces are
used consistently and not mixed.

请注意，此检查仅在制表符或空格使用一致且未混用时才能正常工作。
