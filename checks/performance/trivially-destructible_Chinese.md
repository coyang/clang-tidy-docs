# performance-trivially-destructible

Finds types that could be made trivially-destructible by removing  
out-of-line defaulted destructor declarations.

查找那些可以通过移除类外默认析构函数声明而变为可平凡析构（trivially-destructible）的类型。

```c++
struct A: TrivialType {
  ~A(); // Makes A non-trivially-destructible.
  TrivialType trivial_fields;
};
A::~A() = default;
```

```c++
struct A: TrivialType {
  ~A(); // 使 A 变为不可平凡析构。
  TrivialType trivial_fields;
};
A::~A() = default;
```
