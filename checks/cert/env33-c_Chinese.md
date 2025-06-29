# cert-env33-c

This check flags calls to system(), popen(), and \_popen(), which execute a command processor. It does not flag calls to system() with a null pointer argument, as such a call checks for the presence of a command processor but does not actually attempt to execute a command.

该检查会标记对 system()、popen() 和 \_popen() 的调用，这些函数会执行命令处理器。它不会标记以空指针作为参数的 system() 调用，因为这样的调用只是检查命令处理器是否存在，而不会实际尝试执行命令。

This check corresponds to the CERT C Coding Standard rule ENV33-C. Do not call system().

此检查对应于 CERT C 编码标准规则 ENV33-C：不要调用 system()。
