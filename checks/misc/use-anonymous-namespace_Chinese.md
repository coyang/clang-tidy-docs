# misc-use-anonymous-namespace

Finds instances of static functions or variables declared at global
scope that could instead be moved into an anonymous namespace.

查找在全局作用域中声明的 static 函数或变量，这些声明可以改为使用匿名命名空间。

Anonymous namespaces are the "superior alternative" according to the
C++ Standard. static was proposed for deprecation, but later
un-deprecated to keep C compatibility [1]. static is an overloaded
term with different meanings in different contexts, so it can create
confusion.

根据 C++ 标准，匿名命名空间是更优的替代方案。static 曾被提议废弃，但后来为了保持与 C 的兼容性而取消了废弃 [1]。static 是一个重载术语，在不同上下文中有不同含义，因此可能引起混淆。

The following uses of static will not be diagnosed:

以下使用 static 的情况不会被诊断：

- Functions or variables in header files, since anonymous namespaces
  in headers is considered an antipattern. Allowed header file
  extensions can be configured via the global option
  HeaderFileExtensions.

  头文件中的函数或变量，因为在头文件中使用匿名命名空间被认为是一种反模式。允许的头文件扩展名可以通过全局选项 HeaderFileExtensions 进行配置。

- const or constexpr variables, since they already have implicit
  internal linkage in C++.

  const 或 constexpr 变量，因为它们在 C++ 中已经具有隐式的内部链接属性。

Examples:

示例：

```c++
// Bad
static void foo();
static int x;

// 不推荐
static void foo();
static int x;

// Good
namespace {
  void foo();
  int x;
} // namespace

// 推荐
namespace {
  void foo();
  int x;
} // namespace
```

[1] Undeprecating static  
[1] 取消废弃 static  
https://www.open-std.org/jtc1/sc22/wg21/docs/cwg_defects.html#1012
