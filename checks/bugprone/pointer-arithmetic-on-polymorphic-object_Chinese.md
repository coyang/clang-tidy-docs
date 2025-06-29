# bugprone-pointer-arithmetic-on-polymorphic-object

Finds pointer arithmetic performed on classes that contain a virtual function.

查找在包含虚函数的类上执行指针运算的情况。

Pointer arithmetic on polymorphic objects where the pointer's static type is different from its dynamic type is undefined behavior, as the two types could have different sizes, and thus the vtable pointer could point to an invalid address.

在多态对象上进行指针运算时，如果指针的静态类型与其动态类型不同，则会导致未定义行为，因为这两种类型可能具有不同的大小，从而导致虚函数表（vtable）指针可能指向无效地址。

Finding pointers where the static type contains a virtual member function is a good heuristic, as the pointer is likely to point to a different, derived object.

查找静态类型中包含虚成员函数的指针是一种良好的启发式方法，因为该指针很可能指向一个不同的派生对象。

Example:

示例：

```c++
struct Base {
  virtual ~Base();
  int i;
};

struct Derived : public Base {};

void foo(Base* b) {
  b += 1;
  // warning: pointer arithmetic on class that declares a virtual function can
  // result in undefined behavior if the dynamic type differs from the
  // pointer type
  // 警告：在声明了虚函数的类上进行指针运算，如果动态类型与指针类型不同，可能导致未定义行为
}

int bar(const Derived d[]) {
  return d[1].i; // warning due to pointer arithmetic on polymorphic object
  // 警告：由于在多态对象上进行了指针运算
}

// Making Derived final suppresses the warning
// 将 Derived 声明为 final 可抑制该警告
struct FinalDerived final : public Base {};

int baz(const FinalDerived d[]) {
  return d[1].i; // no warning as FinalDerived is final
  // 无警告，因为 FinalDerived 是 final 类型
}
```

## Options

## 选项

::: option
IgnoreInheritedVirtualFunctions

When true, objects that only inherit a virtual function are not checked. Classes that do not declare a new virtual function are excluded by default, as they make up the majority of false positives.  
Default: false.

当设置为 true 时，仅继承虚函数的对象将不会被检查。默认情况下，不声明新虚函数的类将被排除在外，因为它们构成了大多数误报。  
默认值：false。

```c++
void bar(Base b[], Derived d[]) {
  b += 1; // warning, as Base declares a virtual destructor
          // 警告，因为 Base 声明了一个虚析构函数
  d += 1; // warning only if IgnoreVirtualDeclarationsOnly is set to false
          // 仅当 IgnoreVirtualDeclarationsOnly 设置为 false 时才会警告
}
```

:::

## References

## 参考资料

This check corresponds to the SEI Cert rule CTR56-CPP. Do not use pointer arithmetic on polymorphic objects.

此检查对应于 SEI Cert 规则 CTR56-CPP：不要在多态对象上使用指针运算。

https://wiki.sei.cmu.edu/confluence/display/cplusplus/CTR56-CPP.+Do+not+use+pointer+arithmetic+on+polymorphic+objects
