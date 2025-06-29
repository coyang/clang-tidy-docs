# misc-definitions-in-headers

Finds non-extern non-inline function and variable definitions in header files, which can lead to potential ODR violations in case these headers are included from multiple translation units.

在头文件中查找非 extern 且非 inline 的函数和变量定义，这些定义如果被多个翻译单元包含，可能会导致潜在的 ODR（One Definition Rule，唯一定义规则）违规。

```c++
// Foo.h
int a = 1; // Warning: variable definition.
           // 警告：变量定义。

extern int d; // OK: extern variable.
              // 正确：extern 变量。

namespace N {
  int e = 2; // Warning: variable definition.
             // 警告：变量定义。
}

// Warning: variable definition.
const char* str = "foo";
// 警告：变量定义。

// OK: internal linkage variable definitions are ignored for now.
// Although these might also cause ODR violations, we can be less certain and
// should try to keep the false-positive rate down.
static int b = 1;
const int c = 1;
const char* const str2 = "foo";
constexpr int k = 1;
namespace { int x = 1; }

// 正确：目前忽略具有内部链接的变量定义。
// 尽管这些也可能导致 ODR 违规，但我们无法确定，
// 应尽量降低误报率。

// Warning: function definition.
int g() {
  return 1;
}
// 警告：函数定义。

// OK: inline function definition is allowed to be defined multiple times.
inline int e() {
  return 1;
}
// 正确：inline 函数定义允许被多次定义。

class A {
public:
  int f1() { return 1; } // OK: implicitly inline member function definition is allowed.
                         // 正确：隐式 inline 成员函数定义是允许的。
  int f2();

  static int d;
};

// Warning: not an inline member function definition.
int A::f2() { return 1; }
// 警告：不是 inline 成员函数定义。

// OK: class static data member declaration is allowed.
int A::d = 1;
// 正确：类静态数据成员定义是允许的。

// OK: function template is allowed.
template<typename T>
T f3() {
  T a = 1;
  return a;
}
// 正确：函数模板是允许的。

// Warning: full specialization of a function template is not allowed.
template <>
int f3() {
  int a = 1;
  return a;
}
// 警告：函数模板的完全特化是不允许的。

template <typename T>
struct B {
  void f1();
};

// OK: member function definition of a class template is allowed.
template <typename T>
void B<T>::f1() {}
// 正确：类模板的成员函数定义是允许的。

class CE {
  constexpr static int i = 5; // OK: inline variable definition.
};
// 正确：inline 变量定义。

inline int i = 5; // OK: inline variable definition.
// 正确：inline 变量定义。

constexpr int f10() { return 0; } // OK: constexpr function implies inline.
// 正确：constexpr 函数隐含为 inline。

// OK: C++14 variable templates are inline.
template <class T>
constexpr T pi = T(3.1415926L);
// 正确：C++14 的变量模板是 inline 的。
```

When clang-tidy is invoked with the --fix-notes option, this check provides fixes that automatically add the inline keyword to discovered functions. Please note that the addition of the inline keyword to variables is not currently supported by this check.

当使用 --fix-notes 选项调用 clang-tidy 时，此检查会提供修复建议，自动为发现的函数添加 inline 关键字。请注意，目前此检查不支持为变量添加 inline 关键字。
