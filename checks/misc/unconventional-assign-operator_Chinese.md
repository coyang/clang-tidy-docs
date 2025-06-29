# misc-unconventional-assign-operator

Finds declarations of assign operators with the wrong return and/or argument types and definitions with good return type but wrong return statements.

查找赋值运算符的声明中返回类型和/或参数类型不正确的情况，以及返回类型正确但 return 语句错误的定义。

> - The return type must be Class&.
> - 返回类型必须是 Class&。

> - The assignment may be from the class type by value, const lvalue reference, non-const rvalue reference, or from a completely different type (e.g. int).
> - 赋值操作可以来自类类型的值传递、const 左值引用、非 const 右值引用，或完全不同的类型（例如 int）。

> - Private and deleted operators are ignored.
> - 私有和已删除的运算符将被忽略。

> - The operator must always return \*this.
> - 运算符必须始终返回 \*this。
