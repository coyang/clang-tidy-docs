# android-cloexec-pipe2

This check ensures that pipe2() is called with the O_CLOEXEC flag. The  
check also adds the O_CLOEXEC flag that marks the file descriptor to be  
closed in child processes. Without this flag a sensitive file descriptor  
can be leaked to a child process, potentially into a lower-privileged  
SELinux domain.

此检查确保在调用 pipe2() 时使用了 O_CLOEXEC 标志。  
该检查还会添加 O_CLOEXEC 标志，该标志用于指示在子进程中关闭文件描述符。  
如果没有此标志，敏感的文件描述符可能会泄露到子进程中，  
从而可能进入权限较低的 SELinux 域。

Examples:  
示例：

```c++
pipe2(pipefd, O_NONBLOCK);
```

Suggested replacement:  
建议替换为：

```c++
pipe2(pipefd, O_NONBLOCK | O_CLOEXEC);
```
