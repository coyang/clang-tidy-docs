# bugprone-suspicious-semicolon

Finds most instances of stray semicolons that unexpectedly alter the meaning of the code. More specifically, it looks for if, while, for and for-range statements whose body is a single semicolon, and then analyzes the context of the code (e.g. indentation) in an attempt to determine whether that is intentional.

查找大多数意外改变代码含义的多余分号的情况。更具体地说，它会查找 if、while、for 和 for-range 语句，其主体仅为一个分号，并分析代码的上下文（例如缩进），以尝试判断这是否是有意为之。

```c++
if (x < y);
{
  x++;
}
```

Here the body of the if statement consists of only the semicolon at the end of the first line, and x 将会被递增，无论条件是否满足。

这里 if 语句的主体仅为第一行末尾的分号，因此无论条件是否满足，x 都会被递增。

```c++
while ((line = readLine(file)) != NULL);
  processLine(line);
```

As a result of this code, processLine() will only be called once, when the while loop with the empty body exits with line == NULL. The indentation of the code indicates the intention of the programmer.

由于这段代码的原因，processLine() 只会在 while 循环（其主体为空）以 line == NULL 退出时被调用一次。代码的缩进显示了程序员的意图。

```c++
if (x >= y);
x -= y;
```

While the indentation does not imply any nesting, there is simply no valid reason to have an if statement with an empty body (but it can make sense for a loop). So this check issues a warning for the code above.

虽然缩进并未表示嵌套关系，但使用一个空主体的 if 语句通常没有合理的理由（但对于循环来说可能是合理的）。因此，此检查会对上述代码发出警告。

To solve the issue remove the stray semicolon or in case the empty body is intentional, reflect this using code indentation or put the semicolon in a new line. For example:

为了解决这个问题，可以删除多余的分号；如果空主体是有意为之，则应通过代码缩进来体现，或将分号放在新的一行。例如：

```c++
while (readWhitespace());
  Token t = readNextToken();
```

Here the second line is indented in a way that suggests that it is meant to be the body of the while loop - whose body is in fact empty, because of the semicolon at the end of the first line.

这里第二行的缩进方式暗示它是 while 循环的主体 —— 但实际上由于第一行末尾的分号，循环的主体是空的。

Either remove the indentation from the second line:

可以去掉第二行的缩进：

```c++
while (readWhitespace());
Token t = readNextToken();
```

... or move the semicolon from the end of the first line to a new line:

……或者将第一行末尾的分号移到新的一行：

```c++
while (readWhitespace())
  ;

  Token t = readNextToken();
```

In this case the check will assume that you know what you are doing, and will not raise a warning.

在这种情况下，检查器会认为你是有意为之，因此不会发出警告。
