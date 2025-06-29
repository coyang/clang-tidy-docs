# bugprone-shared-ptr-array-mismatch

Finds initializations of C++ shared pointers to non-array type that are initialized with an array.

查找将 C++ 中非数组类型的 shared_ptr 初始化为数组的情况。

If a shared pointer std::shared_ptr<T> is initialized with a new-expression new T[] the memory is not deallocated correctly. The pointer uses plain delete in this case to deallocate the target memory. Instead a delete[] call is needed. A std::shared_ptr<T[]> calls the correct delete operator.

如果 shared_ptr<T> 使用 new T[] 表达式进行初始化，则内存不会被正确释放。在这种情况下，shared_ptr 会使用普通的 delete 来释放内存，而实际上需要使用 delete[]。而 std::shared_ptr<T[]> 会调用正确的 delete 操作符。

The check offers replacement of shared_ptr<T> to shared_ptr<T[]> if it is used at a single variable declaration (one variable in one statement).

该检查器会在单个变量声明（即一条语句中只声明一个变量）的情况下，建议将 shared_ptr<T> 替换为 shared_ptr<T[]>。

Example:

示例：

```c++
std::shared_ptr<Foo> x(new Foo[10]); // -> std::shared_ptr<Foo[]> x(new Foo[10]);
//                     ^ warning: shared pointer to non-array is initialized with array [bugprone-shared-ptr-array-mismatch]
//                     ^ 警告：将 shared_ptr 指向非数组类型时使用了数组初始化 [bugprone-shared-ptr-array-mismatch]

std::shared_ptr<Foo> x1(new Foo), x2(new Foo[10]); // no replacement
//                                   ^ warning: shared pointer to non-array is initialized with array [bugprone-shared-ptr-array-mismatch]
//                                   ^ 警告：将 shared_ptr 指向非数组类型时使用了数组初始化 [bugprone-shared-ptr-array-mismatch]

std::shared_ptr<Foo> x3(new Foo[10], [](const Foo *ptr) { delete[] ptr; }); // no warning
// 使用自定义删除器 delete[]，因此不会产生警告

struct S {
  std::shared_ptr<Foo> x(new Foo[10]); // no replacement in this case
  //                     ^ warning: shared pointer to non-array is initialized with array [bugprone-shared-ptr-array-mismatch]
  //                     ^ 警告：将 shared_ptr 指向非数组类型时使用了数组初始化 [bugprone-shared-ptr-array-mismatch]
};
```

This check partially covers the CERT C++ Coding Standard rule MEM51-CPP. Properly deallocate dynamically allocated resources  
https://wiki.sei.cmu.edu/confluence/display/cplusplus/MEM51-CPP.+Properly+deallocate+dynamically+allocated+resources

该检查器部分符合 CERT C++ 编码标准规则 MEM51-CPP：正确释放动态分配的资源  
https://wiki.sei.cmu.edu/confluence/display/cplusplus/MEM51-CPP.+Properly+deallocate+dynamically+allocated+resources

However, only the std::shared_ptr case is detected by this check.

然而，该检查器仅检测 std::shared_ptr 的使用情况。
