# android-cloexec-open

A common source of security bugs is code that opens a file without using  
the O_CLOEXEC flag. Without that flag, an opened sensitive file would  
remain open across a fork+exec to a lower-privileged SELinux domain,  
leaking that sensitive data. Open-like functions including open(),  
openat(), and open64() should include O_CLOEXEC in their flags  
argument.

一个常见的安全漏洞来源是打开文件时未使用 O_CLOEXEC 标志的代码。  
如果没有该标志，已打开的敏感文件在执行 fork+exec 到权限较低的 SELinux 域时仍将保持打开状态，  
从而导致敏感数据泄露。类似 open()、openat() 和 open64() 的函数在其 flags 参数中应包含 O_CLOEXEC。

Examples:  
示例：

```c++
open("filename", O_RDWR);
open64("filename", O_RDWR);
openat(0, "filename", O_RDWR);

// becomes
// 修改为

open("filename", O_RDWR | O_CLOEXEC);
open64("filename", O_RDWR | O_CLOEXEC);
openat(0, "filename", O_RDWR | O_CLOEXEC);
```
