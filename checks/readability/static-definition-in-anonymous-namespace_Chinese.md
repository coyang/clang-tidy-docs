# readability-static-definition-in-anonymous-namespace

Finds static function and variable definitions in anonymous namespace.

查找匿名命名空间中的 static 函数和变量定义。

In this case, static is redundant, because anonymous namespace limits the visibility of definitions to a single translation unit.

在这种情况下，static 是多余的，因为匿名命名空间已经将定义的可见性限制在单个翻译单元中。

```c++
namespace {
  static int a = 1; // Warning.
  static const int b = 1; // Warning.
  namespace inner {
    static int c = 1; // Warning.
  }
}
```

The check will apply a fix by removing the redundant static qualifier.

该检查会通过移除多余的 static 修饰符来自动修复问题。
