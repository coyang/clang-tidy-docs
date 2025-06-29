# fuchsia-multiple-inheritance

Warns if a class inherits from multiple classes that are not pure virtual.

如果一个类继承自多个非纯虚类，则会发出警告。

For example, declaring a class that inherits from multiple concrete classes is disallowed:

例如，声明一个类继承自多个具体类是不被允许的：

```c++
class Base_A {
public:
  virtual int foo() { return 0; }
};

class Base_B {
public:
  virtual int bar() { return 0; }
};

// Warning
class Bad_Child1 : public Base_A, Base_B {};
```

A class that inherits from a pure virtual is allowed:

继承自纯虚类的类是被允许的：

```c++
class Interface_A {
public:
  virtual int foo() = 0;
};

class Interface_B {
public:
  virtual int bar() = 0;
};

// No warning
class Good_Child1 : public Interface_A, Interface_B {
  virtual int foo() override { return 0; }
  virtual int bar() override { return 0; }
};
```

See the features disallowed in Fuchsia at  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en>

请参阅 Fuchsia 中不被允许的特性：  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=zh-cn>
