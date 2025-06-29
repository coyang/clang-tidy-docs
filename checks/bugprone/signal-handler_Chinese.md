# bugprone-signal-handler

Finds specific constructs in signal handler functions that can cause undefined behavior. The rules for what is allowed differ between C++ language versions.

查找信号处理函数中可能导致未定义行为的特定结构。允许的规则在不同的 C++ 语言版本之间有所不同。

Checked signal handler rules for C:

C 语言中检查信号处理函数的规则：

- Calls to non-asynchronous-safe functions are not allowed.

- 不允许调用非异步安全的函数。

Checked signal handler rules for up to and including C++14:

C++14 及之前版本中检查信号处理函数的规则：

- Calls to non-asynchronous-safe functions are not allowed.

- 不允许调用非异步安全的函数。

- C++-specific code constructs are not allowed in signal handlers. In other words, only the common subset of C and C++ is allowed to be used.

- 信号处理函数中不允许使用 C++ 特有的代码结构。换句话说，只允许使用 C 和 C++ 的公共子集。

- Calls to functions with non-C linkage are not allowed (including the signal handler itself).

- 不允许调用具有非 C 语言链接方式的函数（包括信号处理函数本身）。

The check is disabled on C++17 and later.

在 C++17 及更高版本中，此检查被禁用。

Asynchronous-safety is determined by comparing the function's name against a set of known functions. In addition, the function must come from a system header include and in a global namespace. The (possible) arguments passed to the function are not checked. Any function that cannot be determined to be asynchronous-safe is assumed to be non-asynchronous-safe by the check, including user functions for which only the declaration is visible. Calls to user-defined functions with visible definitions are checked recursively.

异步安全性是通过将函数名称与一组已知函数进行比较来确定的。此外，函数必须来自系统头文件并位于全局命名空间中。传递给函数的（可能的）参数不会被检查。任何无法确定为异步安全的函数都将被视为非异步安全的函数，包括那些只有声明可见的用户函数。对具有可见定义的用户自定义函数的调用将被递归检查。

This check implements the CERT C Coding Standard rule SIG30-C. Call only asynchronous-safe functions within signal handlers and the rule MSC54-CPP. A signal handler must be a plain old function. It has the alias names cert-sig30-c and cert-msc54-cpp.

此检查实现了 CERT C 编码标准中的规则 SIG30-C：在信号处理函数中仅调用异步安全的函数，以及规则 MSC54-CPP：信号处理函数必须是普通的旧式函数。该检查的别名为 cert-sig30-c 和 cert-msc54-cpp。

## Options

## 选项

::: option
AsyncSafeFunctionSet

Selects which set of functions is considered as asynchronous-safe (and therefore allowed in signal handlers). It can be set to the following values:

选择哪些函数集被视为异步安全（因此允许在信号处理函数中使用）。可以设置为以下值：

minimal

minimal（最小集）

: Selects a minimal set that is defined in the CERT SIG30-C rule. and includes functions abort(), \_Exit(), quick_exit() and signal().

: 选择 CERT SIG30-C 规则中定义的最小函数集。包括函数 abort()、\_Exit()、quick_exit() 和 signal()。

POSIX

POSIX（POSIX 集）

: Selects a larger set of functions that is listed in POSIX.1-2017 (see this link for more information). The following functions are included:  
\_Exit, \_exit, abort, accept, access, aio_error, aio_return, aio_suspend, alarm, bind, cfgetispeed, cfgetospeed, cfsetispeed, cfsetospeed, chdir, chmod, chown, clock_gettime, close, connect, creat, dup, dup2, execl, execle, execv, execve, faccessat, fchdir, fchmod, fchmodat, fchown, fchownat, fcntl, fdatasync, fexecve, ffs, fork, fstat, fstatat, fsync, ftruncate, futimens, getegid, geteuid, getgid, getgroups, getpeername, getpgrp, getpid, getppid, getsockname, getsockopt, getuid, htonl, htons, kill, link, linkat, listen, longjmp, lseek, lstat, memccpy, memchr, memcmp, memcpy, memmove, memset, mkdir, mkdirat, mkfifo, mkfifoat, mknod, mknodat, ntohl, ntohs, open, openat, pause, pipe, poll, posix_trace_event, pselect, pthread_kill, pthread_self, pthread_sigmask, quick_exit, raise, read, readlink, readlinkat, recv, recvfrom, recvmsg, rename, renameat, rmdir, select, sem_post, send, sendmsg, sendto, setgid, setpgid, setsid, setsockopt, setuid, shutdown, sigaction, sigaddset, sigdelset, sigemptyset, sigfillset, sigismember, siglongjmp, signal, sigpause, sigpending, sigprocmask, sigqueue, sigset, sigsuspend, sleep, sockatmark, socket, socketpair, stat, stpcpy, stpncpy, strcat, strchr, strcmp, strcpy, strcspn, strlen, strncat, strncmp, strncpy, strnlen, strpbrk, strrchr, strspn, strstr, strtok_r, symlink, symlinkat, tcdrain, tcflow, tcflush, tcgetattr, tcgetpgrp, tcsendbreak, tcsetattr, tcsetpgrp, time, timer_getoverrun, timer_gettime, timer_settime, times, umask, uname, unlink, unlinkat, utime, utimensat, utimes, wait, waitpid, wcpcpy, wcpncpy, wcscat, wcschr, wcscmp, wcscpy, wcscspn, wcslen, wcsncat, wcsncmp, wcsncpy, wcsnlen, wcspbrk, wcsrchr, wcsspn, wcsstr, wcstok, wmemchr, wmemcmp, wmemcpy, wmemmove, wmemset, write

: 选择 POSIX.1-2017 中列出的更大函数集（详见此链接）。包含以下函数：  
\_Exit、\_exit、abort、accept、access、aio_error、aio_return、aio_suspend、alarm、bind、cfgetispeed、cfgetospeed、cfsetispeed、cfsetospeed、chdir、chmod、chown、clock_gettime、close、connect、creat、dup、dup2、execl、execle、execv、execve、faccessat、fchdir、fchmod、fchmodat、fchown、fchownat、fcntl、fdatasync、fexecve、ffs、fork、fstat、fstatat、fsync、ftruncate、futimens、getegid、geteuid、getgid、getgroups、getpeername、getpgrp、getpid、getppid、getsockname、getsockopt、getuid、htonl、htons、kill、link、linkat、listen、longjmp、lseek、lstat、memccpy、memchr、memcmp、memcpy、memmove、memset、mkdir、mkdirat、mkfifo、mkfifoat、mknod、mknodat、ntohl、ntohs、open、openat、pause、pipe、poll、posix_trace_event、pselect、pthread_kill、pthread_self、pthread_sigmask、quick_exit、raise、read、readlink、readlinkat、recv、recvfrom、recvmsg、rename、renameat、rmdir、select、sem_post、send、sendmsg、sendto、setgid、setpgid、setsid、setsockopt、setuid、shutdown、sigaction、sigaddset、sigdelset、sigemptyset、sigfillset、sigismember、siglongjmp、signal、sigpause、sigpending、sigprocmask、sigqueue、sigset、sigsuspend、sleep、sockatmark、socket、socketpair、stat、stpcpy、stpncpy、strcat、strchr、strcmp、strcpy、strcspn、strlen、strncat、strncmp、strncpy、strnlen、strpbrk、strrchr、strspn、strstr、strtok_r、symlink、symlinkat、tcdrain、tcflow、tcflush、tcgetattr、tcgetpgrp、tcsendbreak、tcsetattr、tcsetpgrp、time、timer_getoverrun、timer_gettime、timer_settime、times、umask、uname、unlink、unlinkat、utime、utimensat、utimes、wait、waitpid、wcpcpy、wcpncpy、wcscat、wcschr、wcscmp、wcscpy、wcscspn、wcslen、wcsncat、wcsncmp、wcsncpy、wcsnlen、wcspbrk、wcsrchr、wcsspn、wcsstr、wcstok、wmemchr、wmemcmp、wmemcpy、wmemmove、wmemset、write

    The function quick_exit is not included in the POSIX list but it is included here in the set of safe functions.

    函数 quick_exit 不包含在 POSIX 列表中，但在此处被包含在安全函数集中。

The default value is POSIX.

默认值为 POSIX。
:::
