# bugprone-argument-comment

Checks that argument comments match parameter names.

检查参数注释是否与参数名称匹配。

The check understands argument comments in the form
/_parameter_name=_/ that are placed right before the argument.

该检查识别形如 /_parameter_name=_/ 的参数注释，并要求其紧贴在参数前。

```c++
void f(bool foo);

...

f(/*bar=*/true);
// warning: argument name 'bar' in comment does not match parameter name 'foo'
```

```c++
void f(bool foo);

...

f(/*bar=*/true);
// 警告：注释中的参数名 'bar' 与实际参数名 'foo' 不匹配
```

The check tries to detect typos and suggest automated fixes for them.

该检查尝试检测拼写错误并提供自动修复建议。

## Options

## 选项

::: option
StrictMode

When false, the check will ignore leading and trailing
underscores and case when comparing names -- otherwise they are taken
into account. Default is false.

当为 false 时，检查在比较名称时会忽略前后下划线和大小写；否则将严格匹配。默认值为 false。
:::

::: option
IgnoreSingleArgument

When true, the check will ignore the single argument.
Default is false.

当为 true 时，检查将忽略仅有的单个参数。默认值为 false。
:::

::: option
CommentBoolLiterals

When true, the check will add argument comments in the
format /_ParameterName=_/ right before the boolean literal argument.
Default is false.

当为 true 时，检查将在布尔字面量参数前添加形如 /_ParameterName=_/ 的注释。默认值为 false。
:::

Before:

之前：

```c++
void foo(bool TurnKey, bool PressButton);

foo(true, false);
```

After:

之后：

```c++
void foo(bool TurnKey, bool PressButton);

foo(/*TurnKey=*/true, /*PressButton=*/false);
```

::: option
CommentIntegerLiterals

When true, the check will add argument comments in the
format /_ParameterName=_/ right before the integer literal argument.
Default is false.

当为 true 时，检查将在整数字面量参数前添加形如 /_ParameterName=_/ 的注释。默认值为 false。
:::

Before:

之前：

```c++
void foo(int MeaningOfLife);

foo(42);
```

After:

之后：

```c++
void foo(int MeaningOfLife);

foo(/*MeaningOfLife=*/42);
```

::: option
CommentFloatLiterals

When true, the check will add argument comments in the
format /_ParameterName=_/ right before the float/double literal
argument. Default is false.

当为 true 时，检查将在浮点/双精度字面量参数前添加形如 /_ParameterName=_/ 的注释。默认值为 false。
:::

Before:

之前：

```c++
void foo(float Pi);

foo(3.14159);
```

After:

之后：

```c++
void foo(float Pi);

foo(/*Pi=*/3.14159);
```

::: option
CommentStringLiterals

When true, the check will add argument comments in the
format /_ParameterName=_/ right before the string literal argument.
Default is false.

当为 true 时，检查将在字符串字面量参数前添加形如 /_ParameterName=_/ 的注释。默认值为 false。
:::

Before:

之前：

```c++
void foo(const char *String);
void foo(const wchar_t *WideString);

foo("Hello World");
foo(L"Hello World");
```

After:

之后：

```c++
void foo(const char *String);
void foo(const wchar_t *WideString);

foo(/*String=*/"Hello World");
foo(/*WideString=*/L"Hello World");
```

::: option
CommentCharacterLiterals

When true, the check will add argument comments in the
format /_ParameterName=_/ right before the character literal argument.
Default is false.

当为 true 时，检查将在字符字面量参数前添加形如 /_ParameterName=_/ 的注释。默认值为 false。
:::

Before:

之前：

```c++
void foo(char *Character);

foo('A');
```

After:

之后：

```c++
void foo(char *Character);

foo(/*Character=*/'A');
```

::: option
CommentUserDefinedLiterals

When true, the check will add argument comments in the
format /_ParameterName=_/ right before the user defined literal
argument. Default is false.

当为 true 时，检查将在用户自定义字面量参数前添加形如 /_ParameterName=_/ 的注释。默认值为 false。
:::

Before:

之前：

```c++
void foo(double Distance);

double operator"" _km(long double);

foo(402.0_km);
```

After:

之后：

```c++
void foo(double Distance);

double operator"" _km(long double);

foo(/*Distance=*/402.0_km);
```

::: option
CommentNullPtrs

When true, the check will add argument comments in the
format /_ParameterName=_/ right before the nullptr literal argument.
Default is false.

当为 true 时，检查将在 nullptr 字面量参数前添加形如 /_ParameterName=_/ 的注释。默认值为 false。
:::

Before:

之前：

```c++
void foo(A* Value);

foo(nullptr);
```

After:

之后：

```c++
void foo(A* Value);

foo(/*Value=*/nullptr);
```
