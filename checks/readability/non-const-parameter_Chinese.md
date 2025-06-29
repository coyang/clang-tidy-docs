# readability-non-const-parameter

The check finds function parameters of a pointer type that could be changed to point to a constant type instead.

该检查会查找可以改为指向常量类型的指针类型函数参数。

When const is used properly, many mistakes can be avoided. Advantages when using const properly:

当正确使用 const 时，可以避免许多错误。正确使用 const 的优点包括：

- prevent unintentional modification of data;
- 防止数据被意外修改；

- get additional warnings such as using uninitialized data;
- 获取额外的警告，例如使用未初始化的数据；

- make it easier for developers to see possible side effects.
- 使开发者更容易识别可能的副作用。

This check is not strict about constness, it only warns when the constness will make the function interface safer.

此检查对 const 的使用并不严格，仅在 const 能使函数接口更安全时发出警告。

```c++
// warning here; the declaration "const char *p" would make the function
// interface safer.
char f1(char *p) {
  return *p;
}

// 此处发出警告；将声明改为 "const char *p" 会使函数接口更安全。
char f1(char *p) {
  return *p;
}

// no warning; the declaration could be more const "const int * const p" but
// that does not make the function interface safer.
int f2(const int *p) {
  return *p;
}

// 无警告；声明可以更具 const 性（如 "const int * const p"），但这不会使函数接口更安全。
int f2(const int *p) {
  return *p;
}

// no warning; making x const does not make the function interface safer
int f3(int x) {
  return x;
}

// 无警告；将 x 声明为 const 并不会使函数接口更安全
int f3(int x) {
  return x;
}

// no warning; Technically, *p can be const ("const struct S *p"). But making
// *p const could be misleading. People might think that it's safe to pass
// const data to this function.
struct S { int *a; int *b; };
int f3(struct S *p) {
  *(p->a) = 0;
}

// 无警告；从技术上讲，*p 可以是 const（如 "const struct S *p"）。但将 *p 设为 const 可能会产生误导，
// 让人误以为可以安全地将 const 数据传入该函数。
struct S { int *a; int *b; };
int f3(struct S *p) {
  *(p->a) = 0;
}

// no warning; p is referenced by an lvalue.
void f4(int *p) {
  int &x = *p;
}

// 无警告；p 被左值引用引用。
void f4(int *p) {
  int &x = *p;
}
```
