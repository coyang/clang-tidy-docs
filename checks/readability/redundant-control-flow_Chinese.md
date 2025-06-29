# readability-redundant-control-flow

# readability-redundant-control-flow（可读性-冗余控制流）

This check looks for procedures (functions returning no value) with  
return statements at the end of the function. Such return statements  
are redundant.

该检查会查找过程（即不返回值的函数）中位于函数末尾的 return 语句。这类 return 语句是冗余的。

Loop statements (for, while, do while) are checked for redundant  
continue statements at the end of the loop body.

循环语句（for、while、do while）也会被检查是否在循环体末尾存在冗余的 continue 语句。

Examples:

示例：

The following function f contains a redundant return statement:

以下函数 f 包含一个冗余的 return 语句：

```c++
extern void g();
void f() {
  g();
  return;
}
```

becomes

变为

```c++
extern void g();
void f() {
  g();
}
```

The following function k contains a redundant continue statement:

以下函数 k 包含一个冗余的 continue 语句：

```c++
void k() {
  for (int i = 0; i < 10; ++i) {
    continue;
  }
}
```

becomes

变为

```c++
void k() {
  for (int i = 0; i < 10; ++i) {
  }
}
```
