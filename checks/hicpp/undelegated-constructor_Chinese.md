# hicpp-undelegated-constructor

This check is an alias for  
该检查是以下规则的别名：

bugprone-undelegated-constructor <../bugprone/undelegated-constructor>{.interpreted-text role="doc"}  
bugprone-undelegated-constructor <../bugprone/undelegated-constructor>{.interpreted-text role="doc"}

Partially implements rule 12.4.5 to find misplaced constructor calls inside a constructor.  
部分实现了规则 12.4.5，用于查找构造函数内部位置不当的构造函数调用。

https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard/special-member-functions  
https://www.perforce.com/resources/qac/high-integrity-cpp-coding-standard/special-member-functions

```c++
struct Ctor {
  Ctor();
  Ctor(int);
  Ctor(int, int);
  Ctor(Ctor *i) {
    // All Ctor() calls result in a temporary object
    // 所有 Ctor() 调用都会生成一个临时对象
    Ctor(); // did you intend to call a delegated constructor?
            // 你是想调用一个委托构造函数吗？
    Ctor(0); // did you intend to call a delegated constructor?
             // 你是想调用一个委托构造函数吗？
    Ctor(1, 2); // did you intend to call a delegated constructor?
                // 你是想调用一个委托构造函数吗？
    foo();
  }
};
```
