# android-cloexec-accept

The usage of accept() is not recommended, it's better to use accept4(). Without this flag, an opened sensitive file descriptor would remain open across a fork+exec to a lower-privileged SELinux domain.

不推荐使用 accept()，建议使用 accept4()。如果不使用该标志，一个已打开的敏感文件描述符在执行 fork+exec 到权限较低的 SELinux 域时将保持打开状态。

Examples:

示例：

```c++
accept(sockfd, addr, addrlen);

// becomes
// 变为

accept4(sockfd, addr, addrlen, SOCK_CLOEXEC);
```
