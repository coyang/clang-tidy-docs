# bugprone-capturing-this-in-member-variable

Finds lambda captures that capture the this pointer and store it as class members without handle the copy and move constructors and the assignments.

查找在 lambda 表达式中捕获 this 指针并将其存储为类成员的情况，而没有正确处理拷贝构造函数、移动构造函数和赋值操作。

Capture this in a lambda and store it as a class member is dangerous because the lambda can outlive the object it captures. Especially when the object is copied or moved, the captured this pointer will be implicitly propagated to the new object. Most of the time, people will believe that the captured this pointer points to the new object, which will lead to bugs.

在 lambda 表达式中捕获 this 并将其作为类成员存储是危险的，因为 lambda 的生命周期可能超过其捕获的对象。特别是在对象被拷贝或移动时，捕获的 this 指针会被隐式地传播到新对象中。大多数情况下，人们会误以为捕获的 this 指针指向的是新对象，这将导致程序出现错误。

```c++
struct C {
  C() : Captured([this]() -> C const * { return this; }) {}
  std::function<C const *()> Captured;
};

void foo() {
  C v1{};
  C v2 = v1; // v2.Captured capture v1's 'this' pointer
  assert(v2.Captured() == v1.Captured()); // v2.Captured capture v1's 'this' pointer
  assert(v2.Captured() == &v2); // assertion failed.
}
```

```c++
struct C {
  C() : Captured([this]() -> C const * { return this; }) {}
  std::function<C const *()> Captured;
};

void foo() {
  C v1{};
  C v2 = v1; // v2.Captured 捕获了 v1 的 'this' 指针
  assert(v2.Captured() == v1.Captured()); // v2.Captured 捕获了 v1 的 'this' 指针
  assert(v2.Captured() == &v2); // 断言失败。
}
```

Possible fixes:

可能的修复方式：

- marking copy and move constructors and assignment operators deleted.
- using class member method instead of class member variable with function object types.
- passing this pointer as parameter

- 将拷贝构造函数、移动构造函数和赋值操作符标记为 deleted。
- 使用类成员函数替代带有函数对象类型的类成员变量。
- 将 this 指针作为参数传递。

## Options

## 选项

::: option
FunctionWrapperTypes

A semicolon-separated list of names of types. Used to specify function wrapper that can hold lambda expressions. Default is [::std::function;::std::move_only_function;::boost::function]{.title-ref}.

以分号分隔的类型名称列表。用于指定可以保存 lambda 表达式的函数包装器类型。默认值为 [::std::function;::std::move_only_function;::boost::function]{.title-ref}。

:::

::: option
BindFunctions

A semicolon-separated list of fully qualified names of functions that can capture this pointer. Default is [::std::bind;::boost::bind;::std::bind_front;::std::bind_back; ::boost::compat::bind_front;::boost::compat::bind_back]{.title-ref}.

以分号分隔的函数全限定名称列表，用于指定可以捕获 this 指针的函数。默认值为 [::std::bind;::boost::bind;::std::bind_front;::std::bind_back; ::boost::compat::bind_front;::boost::compat::bind_back]{.title-ref}。

:::
