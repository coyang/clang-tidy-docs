# readability-static-accessed-through-instance

Checks for member expressions that access static members through instances, and replaces them with uses of the appropriate qualified-id.

检查通过实例访问静态成员的成员表达式，并将其替换为使用适当限定名的形式。

Example:

示例：

The following code:

以下代码：

```c++
struct C {
  static void foo();
  static int x;
  enum { E1 };
  enum E { E2 };
};

C *c1 = new C();
c1->foo();
c1->x;
c1->E1;
c1->E2;
```

is changed to:

将被修改为：

```c++
C *c1 = new C();
C::foo();
C::x;
C::E1;
C::E2;
```

The --fix commandline option provides default support for safe fixes, whereas --fix-notes enables fixes that may replace expressions with side effects, potentially altering the program's behavior.

命令行选项 --fix 提供对安全修复的默认支持，而 --fix-notes 启用可能替换具有副作用表达式的修复，这可能会改变程序的行为。
