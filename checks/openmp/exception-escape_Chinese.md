# openmp-exception-escape

Analyzes OpenMP Structured Blocks and checks that no exception escapes out of the Structured Block it was thrown in.

分析 OpenMP 结构化代码块，并检查是否有异常从其抛出所在的结构化代码块中逸出。

As per the OpenMP specification, a structured block is an executable statement, possibly compound, with a single entry at the top and a single exit at the bottom. Which means, throw may not be used to 'exit' out of the structured block. If an exception is not caught in the same structured block it was thrown in, the behavior is undefined.

根据 OpenMP 规范，结构化代码块是一个可执行语句，可能是复合语句，具有单一的入口（在顶部）和单一的出口（在底部）。这意味着，不能使用 throw 来“退出”结构化代码块。如果一个异常没有在其抛出所在的同一个结构化代码块中被捕获，其行为是未定义的。

FIXME: this check does not model SEH, setjmp/longjmp.

待修复：此检查未对 SEH（结构化异常处理）、setjmp/longjmp 进行建模。

WARNING! This check may be expensive on large source files.

警告！此检查在大型源文件上可能开销较大。

## Options

## 选项

::: option
IgnoredExceptions

Comma-separated list containing type names which are not counted as thrown exceptions in the check. Default value is an empty string.

以逗号分隔的类型名称列表，这些类型在检查中不会被计为抛出的异常。默认值为空字符串。
:::
