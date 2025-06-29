# cppcoreguidelines-avoid-goto

The usage of goto for control flow is error prone and should be replaced with looping constructs. Only forward jumps in nested loops are accepted.

使用 goto 进行控制流容易出错，应该用循环结构替代。仅允许在嵌套循环中进行向前跳转。

This check implements ES.76 from the C++ Core Guidelines and 6.3.1 from High Integrity C++ Coding Standard.

此检查实现了 C++ 核心指南中的 ES.76 和高完整性 C++ 编码标准中的 6.3.1。

For more information on why to avoid programming with goto you can read the famous paper A Case against the GO TO Statement.

关于为何应避免使用 goto 编程的更多信息，请参阅著名论文《A Case against the GO TO Statement》。

The check diagnoses goto for backward jumps in every language mode. These should be replaced with C/C++ looping constructs.

该检查会诊断所有语言模式中向后跳转的 goto。应使用 C/C++ 的循环结构替代这些用法。

```c++
// Bad, handwritten for loop.
int i = 0;
// Jump label for the loop
loop_start:
do_some_operation();

if (i < 100) {
  ++i;
  goto loop_start;
}

// Better
for(int i = 0; i < 100; ++i)
  do_some_operation();
```

```c++
// 错误的，手写的 for 循环。
int i = 0;
// 循环的跳转标签
loop_start:
do_some_operation();

if (i < 100) {
  ++i;
  goto loop_start;
}

// 更好的写法
for(int i = 0; i < 100; ++i)
  do_some_operation();
```

Modern C++ needs goto only to jump out of nested loops.

现代 C++ 仅在需要跳出嵌套循环时才需要使用 goto。

```c++
for(int i = 0; i < 100; ++i) {
  for(int j = 0; j < 100; ++j) {
    if (i * j > 500)
      goto early_exit;
  }
}

early_exit:
some_operation();
```

```c++
for(int i = 0; i < 100; ++i) {
  for(int j = 0; j < 100; ++j) {
    if (i * j > 500)
      goto early_exit;
  }
}

early_exit:
some_operation();
```

All other uses of goto are diagnosed in C++.

C++ 中所有其他用途的 goto 都会被诊断。

## Options

## 选项

::: option
IgnoreMacros

If set to true, the check will not warn if a goto statement is expanded from a macro. Default is false.

如果设置为 true，当 goto 语句是由宏展开生成时，检查将不会发出警告。默认值为 false。
:::
