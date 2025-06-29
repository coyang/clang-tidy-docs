# bugprone-unique-ptr-array-mismatch

Finds initializations of C++ unique pointers to non-array type that are initialized with an array.

查找将 C++ 中的 unique 指针初始化为非数组类型，但却使用数组进行初始化的情况。

If a pointer std::unique_ptr<T> is initialized with a new-expression new T[] the memory is not deallocated correctly. A plain delete is used in this case to deallocate the target memory. Instead a delete[] call is needed. A std::unique_ptr<T[]> uses the correct delete operator. The check does not emit warning if an unique_ptr with user-specified deleter type is used.

如果一个指针 std::unique_ptr<T> 使用 new T[] 表达式进行初始化，则内存不会被正确释放。在这种情况下会使用普通的 delete 来释放目标内存，而实际上应该使用 delete[]。std::unique_ptr<T[]> 使用的是正确的 delete 操作符。如果使用了带有用户自定义删除器类型的 unique_ptr，则此检查不会发出警告。

The check offers replacement of unique_ptr<T> to unique_ptr<T[]> if it is used at a single variable declaration (one variable in one statement).

如果 unique_ptr<T> 在单个变量声明中使用（即一条语句中只声明一个变量），该检查会建议将其替换为 unique_ptr<T[]>。

Example:

示例：

```c++
std::unique_ptr<Foo> x(new Foo[10]); // -> std::unique_ptr<Foo[]> x(new Foo[10]);
//                     ^ warning: unique pointer to non-array is initialized with array
//                     ^ 警告：unique 指针被初始化为非数组类型，但使用了数组

std::unique_ptr<Foo> x1(new Foo), x2(new Foo[10]); // no replacement
//                                   ^ warning: unique pointer to non-array is initialized with array
//                                   ^ 警告：unique 指针被初始化为非数组类型，但使用了数组

D d;
std::unique_ptr<Foo, D> x3(new Foo[10], d); // no warning (custom deleter used)
// 无警告（使用了自定义删除器）

struct S {
  std::unique_ptr<Foo> x(new Foo[10]); // no replacement in this case
  //                     ^ warning: unique pointer to non-array is initialized with array
  //                     ^ 警告：unique 指针被初始化为非数组类型，但使用了数组
};
```

This check partially covers the CERT C++ Coding Standard rule MEM51-CPP. Properly deallocate dynamically allocated resources  
(https://wiki.sei.cmu.edu/confluence/display/cplusplus/MEM51-CPP.+Properly+deallocate+dynamically+allocated+resources)  
However, only the std::unique_ptr case is detected by this check.

此检查部分覆盖了 CERT C++ 编码标准规则 MEM51-CPP：正确释放动态分配的资源  
(https://wiki.sei.cmu.edu/confluence/display/cplusplus/MEM51-CPP.+Properly+deallocate+dynamically+allocated+resources)  
但该检查仅检测 std::unique_ptr 的情况。
