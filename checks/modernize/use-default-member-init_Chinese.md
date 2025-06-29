# modernize-use-default-member-init

This check converts constructors' member initializers into the new default member initializers in C++11. Other member initializers that match the default member initializer are removed. This can reduce repeated code or allow use of '= default'.

此检查会将构造函数中的成员初始化器转换为 C++11 中的新默认成员初始化器。其他与默认成员初始化器相同的成员初始化器将被移除。这可以减少重复代码，或允许使用 '= default'。

```c++
struct A {
  A() : i(5), j(10.0) {}
  A(int i) : i(i), j(10.0) {}
  int i;
  double j;
};

// becomes
// 转换为

struct A {
  A() {}
  A(int i) : i(i) {}
  int i{5};
  double j{10.0};
};
```

::: note

Only converts member initializers for built-in types, enums, and pointers. The readability-redundant-member-init check will remove redundant member initializers for classes.

仅转换内建类型、枚举和指针的成员初始化器。readability-redundant-member-init 检查器将移除类中冗余的成员初始化器。

:::

## Options

## 选项

::: option
UseAssignment

If this option is set to true (default is false), the check will initialize members with an assignment. For example:

如果此选项设置为 true（默认值为 false），该检查器将使用赋值语句来初始化成员。例如：

:::

```c++
struct A {
  A() {}
  A(int i) : i(i) {}
  int i = 5;
  double j = 10.0;
};
```

::: option
IgnoreMacros

If this option is set to true (default is true), the check will not warn about members declared inside macros.

如果此选项设置为 true（默认值为 true），该检查器将不会对宏中声明的成员发出警告。

:::
