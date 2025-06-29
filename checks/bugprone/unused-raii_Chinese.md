# bugprone-unused-raii

Finds temporaries that look like RAII objects.

查找看起来像 RAII（资源获取即初始化）对象的临时变量。

The canonical example for this is a scoped lock.

一个典型的例子是作用域锁（scoped lock）。

```c++
{
  scoped_lock(&global_mutex);
  critical_section();
}
```

The destructor of the scoped_lock is called before the
critical_section is entered, leaving it unprotected.

scoped_lock 的析构函数在进入 critical_section 之前被调用，导致临界区未被保护。

We apply a number of heuristics to reduce the false positive count of
this check:

我们采用了一些启发式方法来减少该检查的误报数量：

- Ignore code expanded from macros. Testing frameworks make heavy use
  of this.

  忽略由宏展开的代码。测试框架大量使用宏。

- Ignore types with trivial destructors. They are very unlikely to be
  RAII objects and there's no difference when they are deleted.

  忽略具有平凡析构函数的类型。这些类型不太可能是 RAII 对象，并且在被销毁时没有区别。

- Ignore objects at the end of a compound statement (doesn't change
  behavior).

  忽略复合语句末尾的对象（不会改变行为）。

- Ignore objects returned from a call.

  忽略从函数调用中返回的对象。
