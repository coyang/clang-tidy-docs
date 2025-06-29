# bugprone-parent-virtual-call

Detects and fixes calls to grand-parent virtual methods instead of calls to overridden parent's virtual methods.

检测并修复对祖父类虚函数的调用，而不是对已被重写的父类虚函数的调用。

```c++
struct A {
  int virtual foo() {...}
};

struct B: public A {
  int foo() override {...}
};

struct C: public B {
  int foo() override { A::foo(); }
//                     ^^^^^^^^
// warning: qualified name A::foo refers to a member overridden in subclass; did you mean 'B'?  [bugprone-parent-virtual-call]
// 警告：限定名称 A::foo 引用了在子类中被重写的成员；你是想写 'B' 吗？ [bugprone-parent-virtual-call]
};
```
