以下是翻译后的 markdown 文件内容，保留了中英文双语格式，中文在英文下方，并以一个空行分隔：

# readability-function-size

[google-readability-function-size]{.title-ref} redirects here as an alias for this check.

[google-readability-function-size]{.title-ref} 是此检查项的别名，重定向至此。

Checks for large functions based on various metrics.

根据多种指标检查函数是否过大。

## Options

## 选项

::: option
LineThreshold

Flag functions exceeding this number of lines. The default is [none]{.title-ref} (ignore the number of lines).

标记超过此行数的函数。默认值为 [none]{.title-ref}（忽略行数）。

:::

::: option
StatementThreshold

Flag functions exceeding this number of statements. This may differ significantly from the number of lines for macro-heavy code. The default is [800]{.title-ref}.

标记超过此语句数的函数。对于宏较多的代码，语句数可能与行数有显著差异。默认值为 [800]{.title-ref}。

:::

::: option
BranchThreshold

Flag functions exceeding this number of control statements. The default is [none]{.title-ref} (ignore the number of branches).

标记超过此控制语句数量的函数。默认值为 [none]{.title-ref}（忽略分支数量）。

:::

::: option
ParameterThreshold

Flag functions that exceed a specified number of parameters. The default is [none]{.title-ref} (ignore the number of parameters).

标记参数数量超过指定值的函数。默认值为 [none]{.title-ref}（忽略参数数量）。

:::

::: option
NestingThreshold

Flag compound statements which create next nesting level after [NestingThreshold]{.title-ref}. This may differ significantly from the expected value for macro-heavy code. The default is [none]{.title-ref} (ignore the nesting level).

标记在超过 [NestingThreshold]{.title-ref} 后创建下一层嵌套的复合语句。对于宏较多的代码，嵌套层级可能与预期值有显著差异。默认值为 [none]{.title-ref}（忽略嵌套层级）。

:::

::: option
VariableThreshold

Flag functions exceeding this number of variables declared in the body. The default is [none]{.title-ref} (ignore the number of variables). Please note that function parameters and variables declared in lambdas, GNU Statement Expressions, and nested class inline functions are not counted.

标记函数体中声明的变量数量超过此值的函数。默认值为 [none]{.title-ref}（忽略变量数量）。请注意，函数参数以及在 lambda 表达式、GNU 语句表达式和嵌套类内联函数中声明的变量不计入此数。

:::

::: option
CountMemberInitAsStmt

When [true]{.title-ref}, count class member initializers in constructors as statements. Default is [true]{.title-ref}.

当设置为 [true]{.title-ref} 时，将构造函数中的类成员初始化器计为语句。默认值为 [true]{.title-ref}。

:::
