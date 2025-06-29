# cppcoreguidelines-special-member-functions

The check finds classes where some but not all of the special member functions are defined.

该检查会发现那些只定义了部分特殊成员函数的类。

By default the compiler defines a copy constructor, copy assignment operator, move constructor, move assignment operator and destructor. The default can be suppressed by explicit user-definitions. The relationship between which functions will be suppressed by definitions of other functions is complicated and it is advised that all five are defaulted or explicitly defined.

默认情况下，编译器会自动生成拷贝构造函数、拷贝赋值运算符、移动构造函数、移动赋值运算符和析构函数。用户可以通过显式定义来抑制这些默认行为。由于某些函数的定义会影响其他函数是否被默认生成，其关系较为复杂，因此建议显式定义或默认所有这五个函数。

Note that defining a function with = delete is considered to be a definition.

请注意，使用 = delete 定义的函数也被视为已定义。

This check implements C.21 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 C.21 条款。

链接: C.21 - https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-five

## Options

## 选项

::: option
AllowSoleDefaultDtor

When set to true (default is false), this check will only trigger on destructors if they are defined and not defaulted.

当设置为 true（默认值为 false）时，该检查仅在析构函数被定义但未使用 default 时触发。

```c++
struct A { // This is fine.
  virtual ~A() = default;
};

struct B { // This is not fine.
  ~B() {}
};

struct C {
  // This is not checked, because the destructor might be defaulted in
  // another translation unit.
  ~C();
};
```

```c++
struct A { // 这是可以接受的。
  virtual ~A() = default;
};

struct B { // 这是不可以接受的。
  ~B() {}
};

struct C {
  // 不会被检查，因为析构函数可能在另一个翻译单元中被默认定义。
  ~C();
};
```

:::

::: option
AllowMissingMoveFunctions

When set to true (default is false), this check doesn't flag classes which define no move operations at all. It still flags classes which define only one of either move constructor or move assignment operator. With this option enabled, the following class won't be flagged:

当设置为 true（默认值为 false）时，该检查不会标记完全未定义移动操作的类。但如果只定义了移动构造函数或移动赋值运算符中的一个，仍会被标记。启用此选项后，以下类将不会被标记：

```c++
struct A {
  A(const A&);
  A& operator=(const A&);
  ~A();
};
```

```c++
struct A {
  A(const A&);
  A& operator=(const A&);
  ~A();
};
```

:::

::: option
AllowMissingMoveFunctionsWhenCopyIsDeleted

When set to true (default is false), this check doesn't flag classes which define deleted copy operations but don't define move operations. This flag is related to Google C++ Style Guide Copyable and Movable Types. With this option enabled, the following class won't be flagged:

当设置为 true（默认值为 false）时，该检查不会标记那些定义了已删除的拷贝操作但未定义移动操作的类。此选项与 Google C++ 风格指南中的“可拷贝和可移动类型”相关。启用此选项后，以下类将不会被标记：

链接: Copyable and Movable Types - https://google.github.io/styleguide/cppguide.html#Copyable_Movable_Types

```c++
struct A {
  A(const A&) = delete;
  A& operator=(const A&) = delete;
  ~A();
};
```

```c++
struct A {
  A(const A&) = delete;
  A& operator=(const A&) = delete;
  ~A();
};
```

:::

::: option
AllowImplicitlyDeletedCopyOrMove

When set to true (default is false), this check doesn't flag classes which implicitly delete copy or move operations. With this option enabled, the following class won't be flagged:

当设置为 true（默认值为 false）时，该检查不会标记那些隐式删除了拷贝或移动操作的类。启用此选项后，以下类将不会被标记：

```c++
struct A : boost::noncopyable {
  ~A() { std::cout << "dtor\n"; }
};
```

```c++
struct A : boost::noncopyable {
  ~A() { std::cout << "dtor\n"; }
};
```

:::

::: option
IgnoreMacros

If set to true, the check will not give warnings for classes defined inside macros. Default is true.

如果设置为 true，则该检查不会对宏中定义的类发出警告。默认值为 true。

:::
