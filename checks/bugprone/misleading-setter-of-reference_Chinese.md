# bugprone-misleading-setter-of-reference

Finds setter-like member functions that take a pointer parameter and set  
a reference member of the same class with the pointed value.

查找类似 setter 的成员函数，这些函数接受一个指针参数，并使用该指针指向的值设置同一类中的引用成员。

The check detects member functions that take a single pointer parameter,  
and contain a single expression statement that dereferences the  
parameter and assigns the result to a data member with a reference type.

该检查会检测那些只接受一个指针参数，并且只包含一个表达式语句的成员函数，该语句对参数进行解引用，并将结果赋值给一个引用类型的数据成员。

The fact that a setter function takes a pointer might cause the belief  
that an internal reference (if it would be a pointer) is changed instead  
of the pointed-to (or referenced) value.

setter 函数接受指针参数这一事实可能会让人误以为它改变了内部引用（如果它是一个指针的话）所指向的对象，而实际上它只是修改了被引用的值。

Example:

示例：

```c++
class MyClass {
  int &InternalRef;  // non-const reference member
                     // 非 const 引用成员
public:
  MyClass(int &Value) : InternalRef(Value) {}

  // Warning: This setter could lead to unintended behaviour.
  // 警告：此 setter 可能导致非预期行为。
  void setRef(int *Value) {
    InternalRef = *Value;  // This assigns to the referenced value, not changing what InternalRef references.
                           // 这实际上是修改了引用的值，而不是改变 InternalRef 所引用的对象。
  }
};

int main() {
  int Value1 = 42;
  int Value2 = 100;
  MyClass X(Value1);

  // This might look like it changes what InternalRef references to,
  // but it actually modifies Value1 to be 100.
  // 这看起来像是改变了 InternalRef 的引用对象，
  // 但实际上是将 Value1 修改为 100。
  X.setRef(&Value2);
}
```

Possible fixes:

可能的修复方式：

- Change the parameter type of the "set" function to non-pointer  
  type (for example, a const reference).
- 将 “set” 函数的参数类型改为非指针类型（例如 const 引用）。

- Change the type of the member variable to a pointer and in the  
  "set" function assign a value to the pointer (without  
  dereference).
- 将成员变量的类型改为指针，并在 “set” 函数中为该指针赋值（不进行解引用）。
