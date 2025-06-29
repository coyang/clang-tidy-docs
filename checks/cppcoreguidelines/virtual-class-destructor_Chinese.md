# cppcoreguidelines-virtual-class-destructor

Finds virtual classes whose destructor is neither public and virtual nor
protected and non-virtual. A virtual class's destructor should be
specified in one of these ways to prevent undefined behavior.

查找虚函数类中析构函数既不是 public 且 virtual，也不是 protected 且非 virtual 的情况。虚函数类的析构函数应以这两种方式之一进行声明，以防止未定义行为。

This check implements
C.35 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的规则 C.35。

Note that this check will diagnose a class with a virtual method
regardless of whether the class is used as a base class or not.

请注意，该检查会诊断所有包含虚函数的方法类，无论该类是否被用作基类。

Fixes are available for user-declared and implicit destructors that are
either public and non-virtual or protected and virtual. No fixes are
offered for private destructors. There, the decision whether to make
them private and virtual or protected and non-virtual depends on the use
case and is thus left to the user.

对于用户声明的或隐式的析构函数，如果它们是 public 且非 virtual，或是 protected 且 virtual，则提供修复建议。对于 private 析构函数则不提供修复建议。在这种情况下，是否将其设为 private 且 virtual，或 protected 且非 virtual，取决于具体使用场景，因此由用户自行决定。

## Example

For example, the following classes/structs get flagged by the check
since they violate guideline C.35:

例如，以下类/结构体会被该检查标记为违规，因为它们违反了规则 C.35：

```c++
struct Foo {        // NOK, protected destructor should not be virtual
  virtual void f();
protected:
  virtual ~Foo(){}
};

class Bar {         // NOK, public destructor should be virtual
  virtual void f();
public:
  ~Bar(){}
};
```

```c++
struct Foo {        // 不符合规范，protected 析构函数不应为 virtual
  virtual void f();
protected:
  virtual ~Foo(){}
};

class Bar {         // 不符合规范，public 析构函数应为 virtual
  virtual void f();
public:
  ~Bar(){}
};
```

This would be rewritten to look like this:

这可以重写为如下形式：

```c++
struct Foo {        // OK, destructor is not virtual anymore
  virtual void f();
protected:
  ~Foo(){}
};

class Bar {         // OK, destructor is now virtual
  virtual void f();
public:
  virtual ~Bar(){}
};
```

```c++
struct Foo {        // 合规，析构函数不再是 virtual
  virtual void f();
protected:
  ~Foo(){}
};

class Bar {         // 合规，析构函数现在是 virtual
  virtual void f();
public:
  virtual ~Bar(){}
};
```
