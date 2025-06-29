# android-cloexec-inotify-init1

`inotify_init1()` should include `IN_CLOEXEC` in its type argument to avoid the file descriptor leakage. Without this flag, an opened sensitive file would remain open across a fork+exec to a lower-privileged SELinux domain.

`inotify_init1()` 应该在其类型参数中包含 `IN_CLOEXEC`，以避免文件描述符泄漏。 如果没有这个标志，一个已打开的敏感文件在执行 fork+exec 到权限较低的 SELinux 域时仍会保持打开状态。

Examples:

示例：

```c++
inotify_init1(IN_NONBLOCK);

// becomes

// 变为

inotify_init1(IN_NONBLOCK | IN_CLOEXEC);
```
