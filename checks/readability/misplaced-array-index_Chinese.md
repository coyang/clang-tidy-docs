# readability-misplaced-array-index

This check warns for unusual array index syntax.

该检查会警告不常见的数组索引语法。

The following code has unusual array index syntax:

以下代码使用了不常见的数组索引语法：

```c++
void f(int *X, int Y) {
  Y[X] = 0;
}
```

becomes

变为

```c++
void f(int *X, int Y) {
  X[Y] = 0;
}
```

The check warns about such unusual syntax for readability reasons:

该检查因可读性原因对这种不常见的语法发出警告：

- There are programmers that are not familiar with this unusual syntax.  
  有些程序员可能不熟悉这种不常见的语法。

- It is possible that variables are mixed up.  
  变量可能被混淆使用。
