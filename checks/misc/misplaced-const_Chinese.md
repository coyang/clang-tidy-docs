# misc-misplaced-const

This check diagnoses when a const qualifier is applied to a typedef/ using to a pointer type rather than to the pointee, because such constructs are often misleading to developers because the const applies to the pointer rather than the pointee.

此检查用于诊断当 const 限定符被应用于指向指针类型的 typedef 或 using，而不是应用于指针所指向的对象（pointee）时的情况，因为这种写法常常会误导开发者，使他们误以为 const 是作用于指针所指向的对象，而实际上是作用于指针本身。

For instance, in the following code, the resulting type is int _ const rather than const int _:

例如，在以下代码中，实际的类型是 int _ const，而不是 const int _：

```c++
typedef int *int_ptr;
void f(const int_ptr ptr) {
  *ptr = 0; // potentially quite unexpectedly the int can be modified here
  ptr = 0; // does not compile
}
```

```c++
typedef int *int_ptr;
void f(const int_ptr ptr) {
  *ptr = 0; // 这里可能出乎意料地修改了 int 的值
  ptr = 0; // 无法编译
}
```

The check does not diagnose when the underlying typedef/using type is a pointer to a const type or a function pointer type. This is because the const qualifier is less likely to be mistaken because it would be redundant (or disallowed) on the underlying pointee type.

当底层的 typedef 或 using 类型是指向 const 类型的指针或函数指针类型时，此检查不会进行诊断。这是因为在这种情况下，const 限定符不太可能被误解，因为它在底层所指向的类型上要么是多余的，要么是不允许的。
