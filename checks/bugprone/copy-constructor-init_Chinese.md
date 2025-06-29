# bugprone-copy-constructor-init

Finds copy constructors where the constructor doesn't call the copy constructor of the base class.

查找拷贝构造函数中未调用基类拷贝构造函数的情况。

```c++
class Copyable {
public:
  Copyable() = default;
  Copyable(const Copyable &) = default;

  int memberToBeCopied = 0;
};

class X2 : public Copyable {
  X2(const X2 &other) {} // Copyable(other) is missing
                         // 缺少 Copyable(other)
};
```

Also finds copy constructors where the constructor of the base class don't have parameter.

还会查找基类构造函数没有参数的拷贝构造函数。

```c++
class X3 : public Copyable {
  X3(const X3 &other) : Copyable() {} // other is missing
                                     // 缺少 other
};
```

Failure to properly initialize base class sub-objects during copy construction can result in undefined behavior, crashes, data corruption, or other unexpected outcomes. The check ensures that the copy constructor of a derived class properly calls the copy constructor of the base class, helping to prevent bugs and improve code quality.

在拷贝构造过程中未正确初始化基类子对象可能会导致未定义行为、程序崩溃、数据损坏或其他意外结果。此检查项确保派生类的拷贝构造函数正确调用基类的拷贝构造函数，从而有助于防止错误并提高代码质量。

Limitations:

限制：

- It won't generate warnings for empty classes, as there are no class members (including base class sub-objects) to worry about.

  对于空类不会产生警告，因为没有类成员（包括基类子对象）需要处理。

- It won't generate warnings for base classes that have copy constructor private or deleted.

  对于拷贝构造函数为私有或已删除的基类不会产生警告。

- It won't generate warnings for base classes that are initialized using other non-default constructor, as this could be intentional.

  对于使用其他非默认构造函数初始化的基类不会产生警告，因为这可能是有意为之。

The check also suggests a fix-its in some cases.

在某些情况下，该检查还会提供自动修复建议。
