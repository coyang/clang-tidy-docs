以下是完整的中英文对照翻译，保留了原始 markdown 和代码块格式：

# readability-identifier-length

This check finds variables and function parameters whose length are too short. The desired name length is configurable.

该检查会查找变量和函数参数名称过短的情况。期望的名称长度是可配置的。

Special cases are supported for loop counters and for exception variable names.

对于循环计数器和异常变量名称，支持特殊情况处理。

## Options

The following options are described below:

以下是可用的选项说明：

> - MinimumVariableNameLength, IgnoredVariableNames  
>   MinimumVariableNameLength（最小变量名长度）、IgnoredVariableNames（忽略的变量名）
> - MinimumParameterNameLength, IgnoredParameterNames  
>   MinimumParameterNameLength（最小参数名长度）、IgnoredParameterNames（忽略的参数名）
> - MinimumLoopCounterNameLength, IgnoredLoopCounterNames  
>   MinimumLoopCounterNameLength（最小循环计数器名长度）、IgnoredLoopCounterNames（忽略的循环计数器名）
> - MinimumExceptionNameLength, IgnoredExceptionVariableNames  
>   MinimumExceptionNameLength（最小异常变量名长度）、IgnoredExceptionVariableNames（忽略的异常变量名）

::: option
MinimumVariableNameLength

All variables (other than loop counter, exception names and function parameters) are expected to have at least a length of MinimumVariableNameLength (default is 3). Setting it to 0 or 1 disables the check entirely.

所有变量（不包括循环计数器、异常变量名和函数参数）都应至少具有 MinimumVariableNameLength 指定的长度（默认值为 3）。将其设置为 0 或 1 将完全禁用此检查。

```c++
int i = 42;    // warns that 'i' is too short
```

```c++
int i = 42;    // 警告：'i' 名称太短
```

This check does not have any fix suggestions in the general case since variable names have semantic value.

由于变量名具有语义价值，因此此检查在一般情况下不会提供修复建议。
:::

::: option
IgnoredVariableNames

Specifies a regular expression for variable names that are to be ignored. The default value is empty, thus no names are ignored.

指定要忽略的变量名的正则表达式。默认值为空，因此不会忽略任何名称。
:::

::: option
MinimumParameterNameLength

All function parameter names are expected to have a length of at least MinimumParameterNameLength (default is 3). Setting it to 0 or 1 disables the check entirely.

所有函数参数名都应至少具有 MinimumParameterNameLength 指定的长度（默认值为 3）。将其设置为 0 或 1 将完全禁用此检查。

```c++
int doubler(int x)   // warns that x is too short
{
   return 2 * x;
}
```

```c++
int doubler(int x)   // 警告：x 名称太短
{
   return 2 * x;
}
```

This check does not have any fix suggestions in the general case since variable names have semantic value.

由于变量名具有语义价值，因此此检查在一般情况下不会提供修复建议。
:::

::: option
IgnoredParameterNames

Specifies a regular expression for parameters that are to be ignored. The default value is ^[n]$ for historical reasons.

指定要忽略的参数名的正则表达式。出于历史原因，默认值为 ^[n]$。
:::

::: option
MinimumLoopCounterNameLength

Loop counter variables are expected to have a length of at least MinimumLoopCounterNameLength characters (default is 2). Setting it to 0 or 1 disables the check entirely.

循环计数器变量应至少具有 MinimumLoopCounterNameLength 指定的字符长度（默认值为 2）。将其设置为 0 或 1 将完全禁用此检查。

```c++
// This warns that 'q' is too short.
for (int q = 0; q < size; ++ q) {
   // ...
}
```

```c++
// 警告：'q' 名称太短。
for (int q = 0; q < size; ++ q) {
   // ...
}
```

:::

::: option
IgnoredLoopCounterNames

Specifies a regular expression for counter names that are to be ignored. The default value is ^[ijk_]$; the first three symbols for historical reasons and the last one since it is frequently used as a "don't care" value, specifically in tools such as Google Benchmark.

指定要忽略的计数器名称的正则表达式。默认值为 ^[ijk_]$；前三个字符是出于历史原因，最后一个字符因为在如 Google Benchmark 等工具中经常用作“无关值”。

```c++
// This does not warn by default, for historical reasons.
for (int i = 0; i < size; ++ i) {
    // ...
}
```

```c++
// 出于历史原因，默认情况下不会警告。
for (int i = 0; i < size; ++ i) {
    // ...
}
```

:::

::: option
MinimumExceptionNameLength

Exception clause variables are expected to have a length of at least MinimumExceptionNameLength (default is 2). Setting it to 0 or 1 disables the check entirely.

异常子句中的变量应至少具有 MinimumExceptionNameLength 指定的长度（默认值为 2）。将其设置为 0 或 1 将完全禁用此检查。

```c++
try {
    // ...
}
// This warns that 'e' is too short.
catch (const std::exception& x) {
    // ...
}
```

```c++
try {
    // ...
}
// 警告：'e' 名称太短。
catch (const std::exception& x) {
    // ...
}
```

:::

::: option
IgnoredExceptionVariableNames

Specifies a regular expression for exception variable names that are to be ignored. The default value is ^[e]$ mainly for historical reasons.

指定要忽略的异常变量名的正则表达式。默认值为 ^[e]$，主要是出于历史原因。

```c++
try {
    // ...
}
// This does not warn by default, for historical reasons.
catch (const std::exception& e) {
    // ...
}
```

```c++
try {
    // ...
}
// 出于历史原因，默认情况下不会警告。
catch (const std::exception& e) {
    // ...
}
```

:::
