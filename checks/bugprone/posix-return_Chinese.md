# bugprone-posix-return

Checks if any calls to pthread*\* or posix*\* functions (except posix_openpt) expect negative return values. These functions return either 0 on success or an errno on failure, which is positive only.

检查是否有对 pthread*\* 或 posix*\* 函数（除了 posix_openpt）的调用期望返回负值。这些函数在成功时返回 0，在失败时返回一个 errno 值，该值始终为正数。

Example buggy usage looks like:

示例中的错误用法如下：

```c
if (posix_fadvise(...) < 0) {
```

This will never happen as the return value is always non-negative. A simple fix could be:

这永远不会发生，因为返回值始终是非负的。一个简单的修复方法如下：

```c
if (posix_fadvise(...) > 0) {
```
