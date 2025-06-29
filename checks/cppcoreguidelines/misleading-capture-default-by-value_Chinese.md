# cppcoreguidelines-misleading-capture-default-by-value

Warns when lambda specify a by-value capture default and capture this.

当 lambda 表达式指定按值捕获默认值并捕获 this 指针时发出警告。

By-value capture defaults in member functions can be misleading about whether data members are captured by value or reference. This occurs because specifying the capture default [=] actually captures the this pointer by value, not the data members themselves. As a result, data members are still indirectly accessed via the captured this pointer, which essentially means they are being accessed by reference. Therefore, even when using [=], data members are effectively captured by reference, which might not align with the user's expectations.

在成员函数中使用按值捕获默认值可能会对数据成员是按值还是按引用捕获产生误导。这是因为指定捕获默认值 [=] 实际上是按值捕获 this 指针，而不是数据成员本身。因此，数据成员仍然是通过捕获的 this 指针间接访问的，这本质上意味着它们是按引用访问的。因此，即使使用 [=]，数据成员实际上也是按引用捕获的，这可能与用户的预期不符。

Examples:

示例：

```c++
struct AClass {
  int member;
  void misleadingLogic() {
    int local = 0;
    member = 0;
    auto f = [=]() mutable {
      local += 1;
      member += 1;
    };
    f();
    // Here, local is 0 but member is 1
    // 此处，local 为 0，但 member 为 1
  }

  void clearLogic() {
    int local = 0;
    member = 0;
    auto f = [this, local]() mutable {
      local += 1;
      member += 1;
    };
    f();
    // Here, local is 0 but member is 1
    // 此处，local 为 0，但 member 为 1
  }
};
```

This check implements F.54 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 F.54 条款。

[F.54](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#f54-when-writing-a-lambda-that-captures-this-or-any-class-data-member-dont-use--default-capture): When writing a lambda that captures this or any class data member, don’t use = default capture.

[F.54](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#f54-when-writing-a-lambda-that-captures-this-or-any-class-data-member-dont-use--default-capture)：在编写捕获 this 或任何类数据成员的 lambda 表达式时，不要使用 = 默认捕获方式。
