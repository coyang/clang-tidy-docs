# bugprone-dynamic-static-initializers

Finds instances of static variables that are dynamically initialized in header files.

查找在头文件中动态初始化的静态变量实例。

This can pose problems in certain multithreaded contexts. For example, when disabling compiler generated synchronization instructions for static variables initialized at runtime (e.g. by -fno-threadsafe-statics), even if a particular project takes the necessary precautions to prevent race conditions during initialization by providing their own synchronization, header files included from other projects may not. Therefore, such a check is helpful for ensuring that disabling compiler generated synchronization for static variable initialization will not cause problems.

这在某些多线程环境中可能会引发问题。例如，当禁用编译器为运行时初始化的静态变量生成的同步指令（例如使用 -fno-threadsafe-statics）时，即使某个项目通过提供自己的同步机制采取了必要的预防措施以防止初始化期间的竞争条件，来自其他项目的头文件可能没有这样做。因此，此类检查有助于确保禁用编译器生成的静态变量初始化同步机制不会引发问题。

Consider the following code:

请参考以下代码：

```c
int foo() {
  static int k = bar();
  return k;
}
```

When synchronization of static initialization is disabled, if two threads both call foo for the first time, there is the possibility that k will be double initialized, creating a race condition.

当静态初始化的同步机制被禁用时，如果两个线程首次同时调用 foo，k 变量可能会被初始化两次，从而产生竞争条件。
