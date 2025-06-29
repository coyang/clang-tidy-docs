# cert-dcl58-cpp

Modification of the std or posix namespace can result in undefined behavior. This check warns for such modifications. The std (or posix) namespace is allowed to be extended with (class or function) template specializations that depend on an user-defined type (a type that is not defined in the standard system headers).

对 std 或 posix 命名空间的修改可能导致未定义行为。此检查会对这类修改发出警告。允许对 std（或 posix）命名空间进行扩展，但仅限于依赖于用户自定义类型（即未在标准系统头文件中定义的类型）的（类或函数）模板特化。

The check detects the following (user provided) declarations in namespace std or posix:

此检查会检测在 std 或 posix 命名空间中出现的以下（用户提供的）声明：

- Anything that is not a template specialization.
- 任何不是模板特化的内容。

- Explicit specializations of any standard library function template or class template, if it does not have any user-defined type as template argument.
- 对标准库函数模板或类模板的显式特化，如果其模板参数中不包含任何用户自定义类型。

- Explicit specializations of any member function of a standard library class template.
- 对标准库类模板的成员函数的显式特化。

- Explicit specializations of any member function template of a standard library class or class template.
- 对标准库类或类模板的成员函数模板的显式特化。

- Explicit or partial specialization of any member class template of a standard library class or class template.
- 对标准库类或类模板的成员类模板的显式或偏特化。

Examples:

示例：

```c++
namespace std {
  int x; // warning: modification of 'std' namespace can result in undefined behavior [cert-dcl58-cpp]
         // 警告：修改 'std' 命名空间可能导致未定义行为 [cert-dcl58-cpp]
}

namespace posix::a { // warning: modification of 'posix' namespace can result in undefined behavior
                     // 警告：修改 'posix' 命名空间可能导致未定义行为
}

template <>
struct ::std::hash<long> { // warning: modification of 'std' namespace can result in undefined behavior
                           // 警告：修改 'std' 命名空间可能导致未定义行为
  unsigned long operator()(const long &K) const {
    return K;
  }
};

struct MyData { long data; };

template <>
struct ::std::hash<MyData> { // no warning: specialization with user-defined type
                             // 无警告：使用用户自定义类型的特化
  unsigned long operator()(const MyData &K) const {
    return K.data;
  }
};

namespace std {
  template <>
  void swap<bool>(bool &a, bool &b); // warning: modification of 'std' namespace can result in undefined behavior
                                     // 警告：修改 'std' 命名空间可能导致未定义行为

  template <>
  bool less<void>::operator()<MyData &&, MyData &&>(MyData &&, MyData &&) const { // warning: modification of 'std' namespace can result in undefined behavior
                                                                                  // 警告：修改 'std' 命名空间可能导致未定义行为
    return true;
  }
}
```

This check corresponds to the CERT C++ Coding Standard rule DCL58-CPP. Do not modify the standard namespaces.

此检查对应于 CERT C++ 编码标准规则 DCL58-CPP：不要修改标准命名空间。

https://www.securecoding.cert.org/confluence/display/cplusplus/DCL58-CPP.+Do+not+modify+the+standard+namespaces
