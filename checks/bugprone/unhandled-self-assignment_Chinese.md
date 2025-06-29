# bugprone-unhandled-self-assignment

[cert-oop54-cpp]{.title-ref} redirects here as an alias for this check.  
[cert-oop54-cpp]{.title-ref} 是此检查项的别名。

For the CERT alias, the [WarnOnlyIfThisHasSuspiciousField]{.title-ref} option is set to [false]{.title-ref}.  
对于 CERT 别名，选项 [WarnOnlyIfThisHasSuspiciousField]{.title-ref} 被设置为 [false]{.title-ref}。

Finds user-defined copy assignment operators which do not protect the code against self-assignment either by checking self-assignment explicitly or using the copy-and-swap or the copy-and-move method.  
查找未对自赋值进行保护的用户自定义拷贝赋值运算符，这些运算符既没有显式检查自赋值，也没有使用拷贝交换或拷贝移动方法。

By default, this check searches only those classes which have any pointer or C array field to avoid false positives. In case of a pointer or a C array, it's likely that self-copy assignment breaks the object if the copy assignment operator was not written with care.  
默认情况下，此检查仅搜索包含指针或 C 数组字段的类，以避免误报。因为如果类中包含指针或 C 数组，而拷贝赋值运算符未谨慎编写，则自拷贝赋值很可能会破坏对象。

See also: OOP54-CPP. Gracefully handle self-copy assignment  
另请参阅：[OOP54-CPP. 优雅地处理自拷贝赋值](https://wiki.sei.cmu.edu/confluence/display/cplusplus/OOP54-CPP.+Gracefully+handle+self-copy+assignment)

A copy assignment operator must prevent that self-copy assignment ruins the object state. A typical use case is when the class has a pointer field and the copy assignment operator first releases the pointed object and then tries to assign it:  
拷贝赋值运算符必须防止自拷贝赋值破坏对象状态。一个典型的场景是类中包含指针字段，而拷贝赋值运算符先释放指针指向的对象，然后再尝试赋值：

```c++
class T {
int* p;

public:
  T(const T &rhs) : p(rhs.p ? new int(*rhs.p) : nullptr) {}
  ~T() { delete p; }

  // ...

  T& operator=(const T &rhs) {
    delete p;
    p = new int(*rhs.p);
    return *this;
  }
};
```

There are two common C++ patterns to avoid this problem. The first is the self-assignment check:  
有两种常见的 C++ 编程模式可以避免此问题。第一种是进行自赋值检查：

```c++
class T {
int* p;

public:
  T(const T &rhs) : p(rhs.p ? new int(*rhs.p) : nullptr) {}
  ~T() { delete p; }

  // ...

  T& operator=(const T &rhs) {
    if(this == &rhs)
      return *this;

    delete p;
    p = new int(*rhs.p);
    return *this;
  }
};
```

The second one is the copy-and-swap method when we create a temporary copy (using the copy constructor) and then swap this temporary object with this:  
第二种是拷贝交换法，即先使用拷贝构造函数创建一个临时副本，然后将该临时对象与 this 交换：

```c++
class T {
int* p;

public:
  T(const T &rhs) : p(rhs.p ? new int(*rhs.p) : nullptr) {}
  ~T() { delete p; }

  // ...

  void swap(T &rhs) {
    using std::swap;
    swap(p, rhs.p);
  }

  T& operator=(const T &rhs) {
    T(rhs).swap(*this);
    return *this;
  }
};
```

There is a third pattern which is less common. Let's call it the copy-and-move method when we create a temporary copy (using the copy constructor) and then move this temporary object into this (needs a move assignment operator):  
还有第三种较少见的模式，我们称之为拷贝移动法，即先使用拷贝构造函数创建一个临时副本，然后将该临时对象移动到 this 中（需要实现移动赋值运算符）：

```c++
class T {
int* p;

public:
  T(const T &rhs) : p(rhs.p ? new int(*rhs.p) : nullptr) {}
  ~T() { delete p; }

  // ...

  T& operator=(const T &rhs) {
    T t = rhs;
    *this = std::move(t);
    return *this;
  }

  T& operator=(T &&rhs) {
    p = rhs.p;
    rhs.p = nullptr;
    return *this;
  }
};
```

## Options

## 选项

::: option  
WarnOnlyIfThisHasSuspiciousField  
仅当存在可疑字段时才警告

When true, the check will warn only if the container class of the copy assignment operator has any suspicious fields (pointer, C array and C++ smart pointer). This option is set to true by default.  
当设置为 true 时，只有当拷贝赋值运算符所在的类包含可疑字段（指针、C 数组或 C++ 智能指针）时，才会触发警告。该选项默认设置为 true。  
:::
