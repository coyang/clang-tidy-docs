# modernize-use-equals-delete

Identifies unimplemented private special member functions, and recommends using = delete for them. Additionally, it recommends relocating any deleted member function from the private to the public section.

识别未实现的私有特殊成员函数，并建议使用 = delete 来标记它们。此外，还建议将任何已删除的成员函数从 private 区域移动到 public 区域。

Before the introduction of C++11, the primary method to effectively "erase" a particular function involved declaring it as private without providing a definition. This approach would result in either a compiler error (when attempting to call a private function) or a linker error (due to an undefined reference).

在 C++11 引入之前，有效“屏蔽”某个函数的主要方法是将其声明为 private 且不提供定义。这种做法会导致编译器错误（当尝试调用私有函数时）或链接器错误（由于引用未定义的函数）。

However, subsequent to the advent of C++11, a more conventional approach emerged for achieving this purpose. It involves flagging functions as = delete and keeping them in the public section of the class.

然而，自从 C++11 出现后，一种更为规范的方法被用于实现这一目的。该方法是将函数标记为 = delete，并将其保留在类的 public 区域中。

To prevent false positives, this check is only active within a translation unit where all other member functions have been implemented. The check will generate partial fixes by introducing = delete, but the user is responsible for manually relocating functions to the public section.

为了避免误报，此检查仅在所有其他成员函数都已实现的翻译单元中启用。该检查会通过引入 = delete 来生成部分修复，但用户需手动将函数移动到 public 区域。

```c++
// Example: bad
class A {
 private:
  A(const A&);
  A& operator=(const A&);
};

// 示例：不推荐写法
class A {
 private:
  A(const A&);
  A& operator=(const A&);
};

// Example: good
class A {
 public:
  A(const A&) = delete;
  A& operator=(const A&) = delete;
};

// 示例：推荐写法
class A {
 public:
  A(const A&) = delete;
  A& operator=(const A&) = delete;
};
```

::: option
IgnoreMacros

If this option is set to true (default is true), the check will not warn about functions declared inside macros.

IgnoreMacros

如果此选项设置为 true（默认值为 true），则该检查不会对宏中声明的函数发出警告。
:::
