以下是翻译后的 markdown 文件内容，保留了中英文双语格式，中文在英文下方，并以一个空行分隔：

# readability-function-cognitive-complexity

Checks function Cognitive Complexity metric.

检查函数的认知复杂度指标。

The metric is implemented as per the COGNITIVE COMPLEXITY by SonarSource specification version 1.2 (19 April 2017).

该指标按照 SonarSource 发布的《COGNITIVE COMPLEXITY》规范版本 1.2（2017 年 4 月 19 日）实现。

## Options

## 选项

::: option
Threshold

Flag functions with Cognitive Complexity exceeding this number. The default is 25.

标记认知复杂度超过该数值的函数。默认值为 25。

:::

::: option
DescribeBasicIncrements

If set to true, then for each function exceeding the complexity threshold the check will issue additional diagnostics on every piece of code (loop, if statement, etc.) which contributes to that complexity. See also the examples below. Default is true.

如果设置为 true，则对于每个超过复杂度阈值的函数，检查器将对每一段导致复杂度增加的代码（如循环、if 语句等）发出额外的诊断信息。参见下方示例。默认值为 true。

:::

::: option
IgnoreMacros

If set to true, the check will ignore code inside macros. Note, that also any macro arguments are ignored, even if they should count to the complexity. As this might change in the future, this option isn't guaranteed to be forward-compatible. Default is false.

如果设置为 true，检查器将忽略宏内部的代码。注意，宏参数也会被忽略，即使它们本应计入复杂度。由于未来可能发生变化，该选项不保证向前兼容。默认值为 false。

:::

## Building blocks

## 构建模块

There are three basic building blocks of a Cognitive Complexity metric:

认知复杂度指标由三个基本构建模块组成：

### Increment

### 增量

The following structures increase the function's Cognitive Complexity metric (by 1):

以下结构会使函数的认知复杂度指标增加（+1）：

- Conditional operators:

  > - if()
  > - else if()
  > - else
  > - cond ? true : false
  - 条件运算符：

    > - if()
    > - else if()
    > - else
    > - cond ? true : false

- switch()
  - switch()

- Loops:

  > - for()
  > - C++11 range-based for()
  > - while()
  > - do while()
  - 循环结构：

    > - for()
    > - C++11 范围 for()
    > - while()
    > - do while()

- catch ()
  - catch ()

- goto LABEL, goto \*(&&LABEL)
  - goto LABEL，goto \*(&&LABEL)

- sequences of binary logical operators:

  > - boolean1 || boolean2
  > - boolean1 && boolean2
  - 二元逻辑运算符序列：

    > - boolean1 || boolean2
    > - boolean1 && boolean2

### Nesting level

### 嵌套层级

While by itself the nesting level does not change the function's Cognitive Complexity metric, it is tracked, and is used by the next, third building block. The following structures increase the nesting level (by 1):

虽然嵌套层级本身不会改变函数的认知复杂度指标，但它会被记录，并在下一个构建模块中使用。以下结构会使嵌套层级增加（+1）：

- Conditional operators:

  > - if()
  > - else if()
  > - else
  > - cond ? true : false
  - 条件运算符：

    > - if()
    > - else if()
    > - else
    > - cond ? true : false

- switch()
  - switch()

- Loops:

  > - for()
  > - C++11 range-based for()
  > - while()
  > - do while()
  - 循环结构：

    > - for()
    > - C++11 范围 for()
    > - while()
    > - do while()

- catch ()
  - catch ()

- Nested functions:

  > - C++11 Lambda
  > - Nested class
  > - Nested struct
  - 嵌套函数：

    > - C++11 Lambda
    > - 嵌套 class
    > - 嵌套 struct

- GNU statement expression
  - GNU 语句表达式

- Apple Block Declaration
  - Apple Block 声明

### Nesting increment

### 嵌套增量

This is where the previous basic building block, Nesting level, matters. The following structures increase the function's Cognitive Complexity metric by the current Nesting level:

此处使用了前一个构建模块“嵌套层级”的值。以下结构会使函数的认知复杂度指标增加当前的嵌套层级值：

- Conditional operators:

  > - if()
  > - cond ? true : false
  - 条件运算符：

    > - if()
    > - cond ? true : false

- switch()
  - switch()

- Loops:

  > - for()
  > - C++11 range-based for()
  > - while()
  > - do while()
  - 循环结构：

    > - for()
    > - C++11 范围 for()
    > - while()
    > - do while()

- catch ()
  - catch ()

## Examples

## 示例

The simplest case. This function has Cognitive Complexity of 0.

最简单的情况。该函数的认知复杂度为 0。

```c++
void function0() {}
```

Slightly better example. This function has Cognitive Complexity of 1.

稍微复杂一点的例子。该函数的认知复杂度为 1。

```c++
int function1(bool var) {
  if(var) // +1, nesting level +1
    return 42;
  return 0;
}
```

Full example. This function has Cognitive Complexity of 3.

完整示例。该函数的认知复杂度为 3。

```c++
int function3(bool var1, bool var2) {
  if(var1) { // +1, nesting level +1
    if(var2)  // +2 (1 + current nesting level of 1), nesting level +1
      return 42;
  }

  return 0;
}
```

In the last example, the check will flag function3 if the option Threshold is set to 2 or smaller. If the option DescribeBasicIncrements is set to true, it will additionally flag the two if statements with the amounts by which they increase to the complexity of the function and the current nesting level.

在最后一个示例中，如果选项 Threshold 设置为 2 或更小，检查器将标记 function3。如果选项 DescribeBasicIncrements 设置为 true，它还将额外标记两个 if 语句，并指出它们对函数复杂度的增量及当前嵌套层级。

## Limitations

## 限制

The metric is implemented with two notable exceptions:

该指标的实现存在两个显著的例外情况：

- preprocessor conditionals (#ifdef, #if, #elif, #else, #endif) are not accounted for.
  - 预处理条件指令（#ifdef、#if、#elif、#else、#endif）不计入复杂度。

- each method in a recursion cycle is not accounted for. It can't be fully implemented, because cross-translational-unit analysis would be needed, which is currently not possible in clang-tidy.
  - 递归循环中的每个方法不计入复杂度。由于需要跨翻译单元分析，而 clang-tidy 当前无法实现，因此无法完全支持此功能。
