# android-cloexec-dup

The usage of dup() is not recommended, it's better to use fcntl(),  
which can set the close-on-exec flag. Otherwise, an opened sensitive  
file would remain open across a fork+exec to a lower-privileged SELinux  
domain.

不推荐使用 dup()，建议使用 fcntl()，  
因为 fcntl() 可以设置 close-on-exec 标志。否则，一个已打开的敏感文件  
在执行 fork+exec 到权限较低的 SELinux 域时仍会保持打开状态。

Examples:  
示例：

```c++
int fd = dup(oldfd);

// becomes
// 变为

int fd = fcntl(oldfd, F_DUPFD_CLOEXEC);
```
