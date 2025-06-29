# android-cloexec-epoll-create1

`epoll_create1()` should include `EPOLL_CLOEXEC` in its type argument to  
avoid the file descriptor leakage. Without this flag, an opened  
sensitive file would remain open across a fork+exec to a  
lower-privileged SELinux domain.

`epoll_create1()` 应该在其类型参数中包含 `EPOLL_CLOEXEC`，以避免文件描述符泄漏。  
如果没有这个标志，一个已打开的敏感文件在执行 fork+exec 到权限较低的 SELinux 域时仍然会保持打开状态。

Examples:  
示例：

```c++
epoll_create1(0);

// becomes
// 变为

epoll_create1(EPOLL_CLOEXEC);
```
