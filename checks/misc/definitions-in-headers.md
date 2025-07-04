# misc-definitions-in-headers

Finds non-extern non-inline function and variable definitions in header
files, which can lead to potential ODR violations in case these headers
are included from multiple translation units.

```c++
// Foo.h
int a = 1; // Warning: variable definition.
extern int d; // OK: extern variable.

namespace N {
  int e = 2; // Warning: variable definition.
}

// Warning: variable definition.
const char* str = "foo";

// OK: internal linkage variable definitions are ignored for now.
// Although these might also cause ODR violations, we can be less certain and
// should try to keep the false-positive rate down.
static int b = 1;
const int c = 1;
const char* const str2 = "foo";
constexpr int k = 1;
namespace { int x = 1; }

// Warning: function definition.
int g() {
  return 1;
}

// OK: inline function definition is allowed to be defined multiple times.
inline int e() {
  return 1;
}

class A {
public:
  int f1() { return 1; } // OK: implicitly inline member function definition is allowed.
  int f2();

  static int d;
};

// Warning: not an inline member function definition.
int A::f2() { return 1; }

// OK: class static data member declaration is allowed.
int A::d = 1;

// OK: function template is allowed.
template<typename T>
T f3() {
  T a = 1;
  return a;
}

// Warning: full specialization of a function template is not allowed.
template <>
int f3() {
  int a = 1;
  return a;
}

template <typename T>
struct B {
  void f1();
};

// OK: member function definition of a class template is allowed.
template <typename T>
void B<T>::f1() {}

class CE {
  constexpr static int i = 5; // OK: inline variable definition.
};

inline int i = 5; // OK: inline variable definition.

constexpr int f10() { return 0; } // OK: constexpr function implies inline.

// OK: C++14 variable templates are inline.
template <class T>
constexpr T pi = T(3.1415926L);
```

When `clang-tidy`{.interpreted-text role="program"} is invoked with the
[\--fix-notes]{.title-ref} option, this check provides fixes that
automatically add the `inline` keyword to discovered functions. Please
note that the addition of the `inline` keyword to variables is not
currently supported by this check.
