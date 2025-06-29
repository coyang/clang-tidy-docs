# google-explicit-constructor

Checks that constructors callable with a single argument and conversion
operators are marked explicit to avoid the risk of unintentional
implicit conversions.

检查可通过单个参数调用的构造函数和转换运算符是否被标记为 explicit，以避免意外的隐式转换风险。

Consider this example:

请看以下示例：

```c++
struct S {
  int x;
  operator bool() const { return true; }
};

bool f() {
  S a{1};
  S b{2};
  return a == b;
}
```

The function will return true, since the objects are implicitly
converted to bool before comparison, which is unlikely to be the
intent.

该函数将返回 true，因为对象在比较之前被隐式转换为 bool，这很可能不是开发者的本意。

The check will suggest inserting explicit before the constructor or
conversion operator declaration. However, copy and move constructors
should not be explicit, as well as constructors taking a single
initializer_list argument.

该检查会建议在构造函数或转换运算符声明前添加 explicit。然而，拷贝构造函数和移动构造函数不应被标记为 explicit，同时接受单个 initializer_list 参数的构造函数也不应如此。

This code:

以下代码：

```c++
struct S {
  S(int a);
  explicit S(const S&);
  operator bool() const;
  ...
```

will become

将会变为：

```c++
struct S {
  explicit S(int a);
  S(const S&);
  explicit operator bool() const;
  ...
```

See  
<https://google.github.io/styleguide/cppguide.html#Explicit_Constructors>

参见  
<https://google.github.io/styleguide/cppguide.html#Explicit_Constructors>
