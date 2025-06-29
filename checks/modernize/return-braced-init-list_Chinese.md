# modernize-return-braced-init-list

Replaces explicit calls to the constructor in a return with a braced initializer list. This way the return type is not needlessly duplicated in the function definition and the return statement.

将 return 语句中对构造函数的显式调用替换为使用大括号的初始化列表。这样可以避免在函数定义和 return 语句中重复书写返回类型。

```c++
Foo bar() {
  Baz baz;
  return Foo(baz);
}

// transforms to:

Foo bar() {
  Baz baz;
  return {baz};
}
```
