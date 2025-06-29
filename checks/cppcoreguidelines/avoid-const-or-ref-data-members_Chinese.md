# cppcoreguidelines-avoid-const-or-ref-data-members

This check warns when structs or classes that are copyable or movable,  
and have const-qualified or reference (lvalue or rvalue) data members.  
Having such members is rarely useful, and makes the class only  
copy-constructible but not copy-assignable.

该检查会在结构体或类是可复制或可移动的，并且包含 const 限定或引用（左值或右值）数据成员时发出警告。  
拥有这类成员通常没有太大意义，并且会导致类只能被拷贝构造，而不能被拷贝赋值。

Examples:

示例：

```c++
// Bad, const-qualified member
// 错误示例，const 限定的数据成员
struct Const {
  const int x;
}

// Good:
// 正确示例：
class Foo {
 public:
  int get() const { return x; }
 private:
  int x;
};

// Bad, lvalue reference member
// 错误示例，左值引用成员
struct Ref {
  int& x;
};

// Good:
// 正确示例：
struct Foo {
  int* x;
  std::unique_ptr<int> x;
  std::shared_ptr<int> x;
  gsl::not_null<int*> x;
};

// Bad, rvalue reference member
// 错误示例，右值引用成员
struct RefRef {
  int&& x;
};
```

This check implements  
C.12 from the C++ Core Guidelines.

该检查实现了 C++ 核心指南中的 C.12 条款。

Further reading: Data members: Never const  
更多阅读：数据成员：永远不要使用 const

https://quuxplusone.github.io/blog/2022/01/23/dont-const-all-the-things/#data-members-never-const
