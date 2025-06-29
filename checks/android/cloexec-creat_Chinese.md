# android-cloexec-creat

The usage of creat() is not recommended, it's better to use open().

不推荐使用 creat()，建议使用 open()。

Examples:

示例：

```c++
int fd = creat(path, mode);

// becomes

int fd = open(path, O_WRONLY | O_CREAT | O_TRUNC | O_CLOEXEC, mode);
```
