# bugprone-compare-pointer-to-member-virtual-function

Detects unspecified behavior about equality comparison between pointer  
to member virtual function and anything other than  
null-pointer-constant.

检测将指向成员虚函数的指针与除空指针常量以外的其他实体进行相等比较时的未定义行为。

```c++
struct A {
  void f1();
  void f2();
  virtual void f3();
  virtual void f4();

  void g1(int);
};

void fn() {
  bool r1 = (&A::f1 == &A::f2);  // ok
  bool r2 = (&A::f1 == &A::f3);  // bugprone
  bool r3 = (&A::f1 != &A::f3);  // bugprone
  bool r4 = (&A::f3 == nullptr); // ok
  bool r5 = (&A::f3 == &A::f4);  // bugprone

  void (A::*v1)() = &A::f3;
  bool r6 = (v1 == &A::f1); // bugprone
  bool r6 = (v1 == nullptr); // ok

  void (A::*v2)() = &A::f2;
  bool r7 = (v2 == &A::f1); // false positive, but potential risk if assigning other value to v2.

  void (A::*v3)(int) = &A::g1;
  bool r8 = (v3 == &A::g1); // ok, no virtual function match void(A::*)(int) signature.
}
```

Provide warnings on equality comparisons involve pointers to member  
virtual function or variables which is potential pointer to member  
virtual function and any entity other than a null-pointer constant.

当将指向成员虚函数的指针或可能是指向成员虚函数的变量与除空指针常量以外的实体进行相等比较时，发出警告。

In certain compilers, virtual function addresses are not conventional  
pointers but instead consist of offsets and indexes within a virtual  
function table (vtable). Consequently, these pointers may vary between  
base and derived classes, leading to unpredictable behavior when  
compared directly. This issue becomes particularly challenging when  
dealing with pointers to pure virtual functions, as they may not even  
have a valid address, further complicating comparisons.

在某些编译器中，虚函数的地址并不是传统意义上的指针，而是由虚函数表（vtable）中的偏移量和索引组成。因此，这些指针在基类和派生类之间可能不同，直接比较时会导致不可预测的行为。这个问题在处理纯虚函数的指针时尤为复杂，因为它们可能根本没有有效地址，从而使比较更加困难。

Instead, it is recommended to utilize the typeid operator or other  
appropriate mechanisms for comparing objects to ensure robust and  
predictable behavior in your codebase. By heeding this detection and  
adopting a more reliable comparison method, you can mitigate potential  
issues related to unspecified behavior, especially when dealing with  
pointers to member virtual functions or pure virtual functions, thereby  
improving the overall stability and maintainability of your code. In  
scenarios involving pointers to member virtual functions, it's only  
advisable to employ nullptr for comparisons.

因此，建议使用 typeid 运算符或其他合适的机制来比较对象，以确保代码库中行为的健壮性和可预测性。通过遵循此检测并采用更可靠的比较方法，可以减轻与未定义行为相关的潜在问题，特别是在处理指向成员虚函数或纯虚函数的指针时，从而提高代码的整体稳定性和可维护性。在涉及指向成员虚函数的指针的场景中，仅建议使用 nullptr 进行比较。

## Limitations

Does not analyze values stored in a variable. For variable, only analyze  
all virtual methods in the same class or struct and diagnose when  
assigning a pointer to member virtual function to this variable is  
possible.

不会分析存储在变量中的值。对于变量，仅分析同一 class 或 struct 中的所有虚函数，并在可能将指向成员虚函数的指针赋值给该变量时进行诊断。
