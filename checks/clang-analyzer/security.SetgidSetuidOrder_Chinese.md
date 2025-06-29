# clang-analyzer-security.SetgidSetuidOrder

The checker checks for sequences of setuid(getuid()) and setgid(getgid()) calls (in this order). If such a sequence is found and there is no other privilege-changing function call (seteuid, setreuid, setresuid and the GID versions of these) in between, a warning is generated. The checker finds only exactly setuid(getuid()) calls (and the GID versions), not for example if the result of getuid() is stored in a variable.

检查器会检查 setuid(getuid()) 和 setgid(getgid()) 的调用序列（按此顺序）。如果发现这样的调用序列，并且中间没有其他更改权限的函数调用（如 seteuid、setreuid、setresuid 及其对应的 GID 版本），则会生成一个警告。该检查器仅检测完全形式的 setuid(getuid()) 调用（以及对应的 GID 版本），例如不会检测将 getuid() 的结果存储在变量中的情况。

The clang-analyzer-security.SetgidSetuidOrder check is an alias, please see Clang Static Analyzer Available Checkers for more information.

clang-analyzer-security.SetgidSetuidOrder 检查项是一个别名，请参阅 Clang 静态分析器可用检查器 了解更多信息。
