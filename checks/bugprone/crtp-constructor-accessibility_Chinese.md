# bugprone-crtp-constructor-accessibility

Detects error-prone Curiously Recurring Template Pattern usage, when the  
CRTP can be constructed outside itself and the derived class.

检测容易出错的“奇异递归模板模式”（CRTP）用法，即 CRTP 可以在其自身和派生类之外被构造的情况。

The CRTP is an idiom, in which a class derives from a template class,  
where itself is the template argument. It should be ensured that if a  
class is intended to be a base class in this idiom, it can only be  
instantiated if the derived class is its template argument.

CRTP 是一种编程习惯用法，其中一个类继承自一个以自身作为模板参数的模板类。应确保如果一个类打算作为这种模式中的基类，它只能在派生类作为其模板参数时被实例化。

Example:  
示例：

```c++
template <typename T> class CRTP {
private:
  CRTP() = default;
  friend T;
};

class Derived : CRTP<Derived> {};
```

Below can be seen some common mistakes that will allow the breaking of  
the idiom.

以下是一些常见的错误用法，它们会破坏该模式的正确性。

If the constructor of a class intended to be used in a CRTP is public,  
then it allows users to construct that class on its own.

如果一个打算用于 CRTP 的类的构造函数是 public 的，那么用户就可以单独构造该类的实例。

Example:  
示例：

```c++
template <typename T> class CRTP {
public:
  CRTP() = default;
};

class Good : CRTP<Good> {};
Good GoodInstance;

CRTP<int> BadInstance;
```

If the constructor is protected, the possibility of an accidental  
instantiation is prevented, however it can fade an error, when a  
different class is used as the template parameter instead of the derived  
one.

如果构造函数是 protected 的，可以防止意外实例化，但当使用了非派生类作为模板参数时，可能会掩盖错误。

Example:  
示例：

```c++
template <typename T> class CRTP {
protected:
  CRTP() = default;
};

class Good : CRTP<Good> {};
Good GoodInstance;

class Bad : CRTP<Good> {};
Bad BadInstance;
```

To ensure that no accidental instantiation happens, the best practice is  
to make the constructor private and declare the derived class as friend.  
Note that as a tradeoff, this also gives the derived class access to  
every other private members of the CRTP. However, constructors can still  
be public or protected if they are deleted.

为了确保不会发生意外实例化，最佳实践是将构造函数设为 private，并将派生类声明为 friend。  
注意，这样做的代价是派生类可以访问 CRTP 的所有其他私有成员。  
不过，如果构造函数被删除（deleted），它仍然可以是 public 或 protected 的。

Example:  
示例：

```c++
template <typename T> class CRTP {
  CRTP() = default;
  friend T;
};

class Good : CRTP<Good> {};
Good GoodInstance;

class Bad : CRTP<Good> {};
Bad CompileTimeError;

CRTP<int> AlsoCompileTimeError;
```

Limitations:  
限制：

- The check is not supported below C++11  
  此检查不支持 C++11 以下的版本

- The check does not handle when the derived class is passed as a  
  variadic template argument  
  此检查不处理派生类作为可变模板参数传入的情况

- Accessible functions that can construct the CRTP, like factory  
  functions are not checked  
  能够构造 CRTP 的可访问函数（如工厂函数）不会被检查

The check also suggests a fix-its in some cases.  
在某些情况下，该检查还会提供修复建议。
