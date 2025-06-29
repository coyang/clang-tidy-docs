# android-cloexec-memfd-create

memfd_create() should include MFD_CLOEXEC in its type argument to avoid the file descriptor leakage. Without this flag, an opened sensitive file would remain open across a fork+exec to a lower-privileged SELinux domain.

memfd_create() 应该在其 type 参数中包含 MFD_CLOEXEC，以避免文件描述符泄漏。如果没有这个标志，一个已打开的敏感文件在执行 fork+exec 到权限较低的 SELinux 域时仍将保持打开状态。

Examples:

示例：

```c++
memfd_create(name, MFD_ALLOW_SEALING);

// becomes
// 变为

memfd_create(name, MFD_ALLOW_SEALING | MFD_CLOEXEC);
```
