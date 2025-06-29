# bugprone-lambda-function-name

Checks for attempts to get the name of a function from within a lambda expression. The name of a lambda is always something like operator(), which is almost never what was intended.

检查在 lambda 表达式中尝试获取函数名称的行为。lambda 的名称始终类似于 operator()，这几乎从来不是开发者的本意。

Example:  
示例：

```c++
void FancyFunction() {
  [] { printf("Called from %s\n", __func__); }();
  [] { printf("Now called from %s\n", __FUNCTION__); }();
}
```

Output:  
输出：

    Called from operator()
    Now called from operator()

Likely intended output:  
可能期望的输出：

    Called from FancyFunction
    Now called from FancyFunction

## Options

## 选项

::: option  
IgnoreMacros

The value true specifies that attempting to get the name of a function from within a macro should not be diagnosed. The default value is false.

值为 true 表示在宏中尝试获取函数名称的行为不应被诊断。默认值为 false。
:::
