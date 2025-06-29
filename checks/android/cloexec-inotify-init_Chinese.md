# android-cloexec-inotify-init

The usage of inotify_init() is not recommended, it's better to use inotify_init1().

不推荐使用 inotify_init()，建议使用 inotify_init1()。

Examples:

示例：

```c++
inotify_init();

// becomes

inotify_init1(IN_CLOEXEC);
```

```c++
inotify_init();

// 变为

inotify_init1(IN_CLOEXEC);
```
