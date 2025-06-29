# cppcoreguidelines-slicing

Flags slicing of member variables or vtable. Slicing happens when  
copying a derived object into a base object: the members of the derived  
object (both member variables and virtual member functions) will be  
discarded. This can be misleading especially for member function  
slicing, for example:

标记成员变量或虚函数表（vtable）被切片的情况。切片发生在将派生类对象复制到基类对象中时：派生类对象的成员（包括成员变量和虚成员函数）将被丢弃。这种行为可能具有误导性，尤其是在成员函数被切片的情况下，例如：

```c++
struct B { int a; virtual int f(); };
struct D : B { int b; int f() override; };

void use(B b) {  // Missing reference, intended?
  b.f();  // Calls B::f.
}

D d;
use(d);  // Slice.
```

This check implements  
ES.63 and  
C.145 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的  
ES.63 和  
C.145。
