# bugprone-misplaced-operator-in-strlen-in-alloc

Finds cases where 1 is added to the string in the argument to strlen(), strnlen(), strnlen_s(), wcslen(), wcsnlen(), and wcsnlen_s() instead of the result and the value is used as an argument to a memory allocation function (malloc(), calloc(), realloc(), alloca()) or the new[] operator in C++. The check detects error cases even if one of these functions (except the new[] operator) is called by a constant function pointer. Cases where 1 is added both to the parameter and the result of the strlen()-like function are ignored, as are cases where the whole addition is surrounded by extra parentheses.

查找在调用 strlen()、strnlen()、strnlen_s()、wcslen()、wcsnlen() 和 wcsnlen_s() 等函数时，将 1 加到了字符串参数上而不是函数返回值上的情况，并且该值被用作内存分配函数（malloc()、calloc()、realloc()、alloca()）或 C++ 中的 new[] 运算符的参数。该检查能够检测出错误用法，即使这些函数（除了 new[] 运算符）是通过常量函数指针调用的。如果在参数和 strlen() 类似函数的返回值上都加了 1，或者整个加法表达式被额外的括号包裹，则不会被报告。

C 语言示例代码：

```c
void bad_malloc(char *str) {
  char *c = (char*) malloc(strlen(str + 1));
}
```

建议的修复方式是将 1 加到 strlen() 的返回值上，而不是其参数上。在上述示例中，修复后的代码如下：

```c
char *c = (char*) malloc(strlen(str) + 1);
```

C++ 示例代码：

```c++
void bad_new(char *str) {
  char *c = new char[strlen(str + 1)];
}
```

与 C 语言中 malloc() 函数的情况类似，建议的修复方式是将 1 加到 strlen() 的返回值上，而不是其参数上。在上述示例中，修复后的代码如下：

```c++
char *c = new char[strlen(str) + 1];
```

用于抑制诊断的示例：

```c
void bad_malloc(char *str) {
  char *c = (char*) malloc(strlen((str + 1)));
}
```
