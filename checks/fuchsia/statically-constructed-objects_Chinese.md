# fuchsia-statically-constructed-objects

Warns if global, non-trivial objects with static storage are constructed, unless the object is statically initialized with a constexpr constructor or has no explicit constructor.

如果全局的、具有静态存储的非平凡对象被构造，且该对象不是通过 constexpr 构造函数进行静态初始化，或没有显式构造函数，则会发出警告。

For example:

例如：

```c++
class A {};

class B {
public:
  B(int Val) : Val(Val) {}
private:
  int Val;
};

class C {
public:
  constexpr C(int Val) : Val(Val) {}
  C(int Val1, int Val2) : Val(Val1+Val2) {}

private:
  int Val;
};

static A a;         // No warning, as there is no explicit constructor
                   // 不会警告，因为没有显式构造函数

static C c(0);      // No warning, as constructor is constexpr
                   // 不会警告，因为构造函数是 constexpr

static B b(0);      // Warning, as constructor is not constexpr
                   // 会警告，因为构造函数不是 constexpr

static C c2(0, 1);  // Warning, as constructor is not constexpr
                   // 会警告，因为构造函数不是 constexpr

static int i;       // No warning, as it is trivial
                   // 不会警告，因为这是一个平凡类型

extern int get_i();
static C c3(get_i());// Warning, as the constructor is dynamically initialized
                    // 会警告，因为构造函数是动态初始化的
```

See the features disallowed in Fuchsia at  
请参阅 Fuchsia 中不允许使用的特性：  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en>
