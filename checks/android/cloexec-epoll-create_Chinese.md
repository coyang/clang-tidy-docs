# android-cloexec-epoll-create

The usage of epoll_create() is not recommended, it's better to use epoll_create1(), which allows close-on-exec.

不推荐使用 epoll_create()，更好的做法是使用 epoll_create1()，它支持 close-on-exec（执行时关闭）。

Examples:

示例：

```c++
epoll_create(size);

// becomes

epoll_create1(EPOLL_CLOEXEC);
```

```c++
epoll_create(size);

// 变为

epoll_create1(EPOLL_CLOEXEC);
```
