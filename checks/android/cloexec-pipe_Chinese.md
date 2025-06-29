# android-cloexec-pipe

This check detects usage of pipe(). Using pipe() is not recommended, pipe2() is the suggested replacement. The check also adds the O_CLOEXEC flag that marks the file descriptor to be closed in child processes. Without this flag a sensitive file descriptor can be leaked to a child process, potentially into a lower-privileged SELinux domain.

此检查用于检测 pipe() 的使用。由于不推荐使用 pipe()，建议使用 pipe2() 作为替代。该检查还会添加 O_CLOEXEC 标志，该标志用于指示在子进程中关闭文件描述符。如果没有这个标志，敏感的文件描述符可能会泄露到子进程中，进而可能进入权限较低的 SELinux 域。

Examples:

示例：

```c++
pipe(pipefd);
```

Suggested replacement:

建议替代写法：

```c++
pipe2(pipefd, O_CLOEXEC);
```
