# cppcoreguidelines-prefer-member-initializer

Finds member initializations in the constructor body which can be
converted into member initializers of the constructor instead. This not
only improves the readability of the code but also positively affects
its performance. Class-member assignments inside a control statement or
following the first control statement are ignored.

在构造函数体中查找可以转换为构造函数成员初始化器的成员初始化。这不仅提高了代码的可读性，还能对性能产生积极影响。位于控制语句内部或紧随第一个控制语句之后的类成员赋值将被忽略。

This check implements
C.49 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的规则 C.49。

C.49: Prefer initialization to assignment in constructors  
C.49：在构造函数中优先使用初始化而非赋值  
https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c49-prefer-initialization-to-assignment-in-constructors

Please note, that this check does not enforce rule
C.48 from the C++ Core Guidelines. For that purpose see check
modernize-use-default-member-init.

请注意，此检查不会强制执行 C++ 核心指南中的规则 C.48。如需此功能，请参阅检查项 modernize-use-default-member-init。

C.48: Prefer in-class initializers to member initializers in constructors for constant initializers  
C.48：对于常量初始化器，优先使用类内初始化器而非构造函数中的成员初始化器  
https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c48-prefer-in-class-initializers-to-member-initializers-in-constructors-for-constant-initializers

modernize-use-default-member-init  
modernize-use-default-member-init  
<../modernize/use-default-member-init>

## Example 1

## 示例 1

```c++
class C {
  int n;
  int m;
public:
  C() {
    n = 1; // Literal in default constructor
    if (dice())
      return;
    m = 1;
  }
};
```

Here n can be initialized in the constructor initializer list, unlike
m, as m's initialization follows a control statement (if):

在这里，n 可以在构造函数的初始化列表中进行初始化，而 m 不可以，因为 m 的初始化出现在控制语句（if）之后：

```c++
class C {
  int n;
  int m;
public:
  C(): n(1) {
    if (dice())
      return;
    m = 1;
  }
};
```

## Example 2

## 示例 2

```c++
class C {
  int n;
  int m;
public:
  C(int nn, int mm) {
    n = nn; // Neither default constructor nor literal
    if (dice())
      return;
    m = mm;
  }
};
```

Here n can be initialized in the constructor initializer list, unlike
m, as m's initialization follows a control statement (if):

在这里，n 可以在构造函数的初始化列表中进行初始化，而 m 不可以，因为 m 的初始化出现在控制语句（if）之后：

```c++
C(int nn, int mm) : n(nn) {
  if (dice())
    return;
  m = mm;
}
```
