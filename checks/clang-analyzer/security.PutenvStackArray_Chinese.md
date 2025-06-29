# clang-analyzer-security.PutenvStackArray

Finds calls to the putenv function which pass a pointer to a stack-allocated (automatic) array as the argument. Function putenv does not copy the passed string, only a pointer to the data is stored and this data can be read even by other threads. Content of a stack-allocated array is likely to be overwritten after exiting from the function.

检测传递指向栈分配（自动）数组的指针作为参数的 putenv 函数调用。putenv 函数不会复制传入的字符串，只会存储数据的指针，并且这些数据甚至可以被其他线程读取。栈分配数组的内容在函数退出后很可能会被覆盖。

The clang-analyzer-security.PutenvStackArray check is an alias, please see Clang Static Analyzer Available Checkers for more information.

clang-analyzer-security.PutenvStackArray 检查项是一个别名，请参阅 Clang 静态分析器可用检查项 了解更多信息。
