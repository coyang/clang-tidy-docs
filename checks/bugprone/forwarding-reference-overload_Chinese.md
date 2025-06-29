# bugprone-forwarding-reference-overload

# bugprone-forwarding-reference-overload（易出错的转发引用重载）

The check looks for perfect forwarding constructors that can hide copy or move constructors. If a non const lvalue reference is passed to the constructor, the forwarding reference parameter will be a better match than the const reference parameter of the copy constructor, so the perfect forwarding constructor will be called, which can be confusing. For detailed description of this issue see: Scott Meyers, Effective Modern C++, Item 26.

该检查项会查找可能隐藏拷贝构造函数或移动构造函数的完美转发构造函数。如果向构造函数传递一个非常量左值引用，转发引用参数会比拷贝构造函数中的常量引用参数更匹配，因此会调用完美转发构造函数，这可能会导致困惑。关于该问题的详细描述，请参见：Scott Meyers 所著《Effective Modern C++》中的第 26 条。

Consider the following example:

请参考以下示例：

```c++
class Person {
public:
  // C1: perfect forwarding ctor
  // C1：完美转发构造函数
  template<typename T>
  explicit Person(T&& n) {}

  // C2: perfect forwarding ctor with parameter default value
  // C2：带有参数默认值的完美转发构造函数
  template<typename T>
  explicit Person(T&& n, int x = 1) {}

  // C3: perfect forwarding ctor guarded with enable_if
  // C3：使用 enable_if 保护的完美转发构造函数
  template<typename T, typename X = enable_if_t<is_special<T>, void>>
  explicit Person(T&& n) {}

  // C4: variadic perfect forwarding ctor guarded with enable_if
  // C4：使用 enable_if 保护的可变参数完美转发构造函数
  template<typename... A,
    enable_if_t<is_constructible_v<tuple<string, int>, A&&...>, int> = 0>
  explicit Person(A&&... a) {}

  // C5: perfect forwarding ctor guarded with requires expression
  // C5：使用 requires 表达式保护的完美转发构造函数
  template<typename T>
  requires requires { is_special<T>; }
  explicit Person(T&& n) {}

  // C6: perfect forwarding ctor guarded with concept requirement
  // C6：使用 concept 要求保护的完美转发构造函数
  template<Special T>
  explicit Person(T&& n) {}

  // (possibly compiler generated) copy ctor
  // （可能由编译器生成的）拷贝构造函数
  Person(const Person& rhs);
};
```

The check warns for constructors C1 and C2, because those can hide copy and move constructors. We suppress warnings if the copy and the move constructors are both disabled (deleted or private), because there is nothing the perfect forwarding constructor could hide in this case. We also suppress warnings for constructors like C3-C6 that are guarded with an enable_if or a concept, assuming the programmer was aware of the possible hiding.

该检查会对构造函数 C1 和 C2 发出警告，因为它们可能会隐藏拷贝构造函数和移动构造函数。如果拷贝构造函数和移动构造函数都被禁用（被删除或设为私有），我们会抑制警告，因为在这种情况下完美转发构造函数不会隐藏任何内容。我们也会对像 C3-C6 这样使用 enable_if 或 concept 进行保护的构造函数抑制警告，假设程序员已经意识到可能存在的隐藏问题。

## Background

## 背景

For deciding whether a constructor is guarded with enable_if, we consider the types of the constructor parameters, the default values of template type parameters and the types of non-type template parameters with a default literal value. If any part of these types is std::enable_if or std::enable_if_t, we assume the constructor is guarded.

为了判断一个构造函数是否使用 enable_if 进行了保护，我们会考虑构造函数参数的类型、模板类型参数的默认值以及具有默认字面值的非类型模板参数的类型。如果这些类型的任何部分包含 std::enable_if 或 std::enable_if_t，我们就认为该构造函数是受保护的。
