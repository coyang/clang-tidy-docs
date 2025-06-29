# bugprone-misplaced-pointer-arithmetic-in-alloc

Finds cases where an integer expression is added to or subtracted from  
the result of a memory allocation function (malloc(), calloc(),  
realloc(), alloca()) instead of its argument. The check detects  
error cases even if one of these functions is called by a constant  
function pointer.

查找将整数表达式加到或减去内存分配函数（malloc()、calloc()、realloc()、alloca()）的返回值，而不是其参数的情况。即使这些函数是通过常量函数指针调用的，该检查也能检测到错误情况。

Example code:

示例代码：

```c
void bad_malloc(int n) {
  char *p = (char*) malloc(n) + 10;
}
```

The suggested fix is to add the integer expression to the argument of  
malloc and not to its result. In the example above the fix would be

建议的修复方法是将整数表达式加到 malloc 的参数上，而不是其返回值上。在上面的示例中，修复方式如下：

```c
char *p = (char*) malloc(n + 10);
```
