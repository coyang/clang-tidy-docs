# linuxkernel-must-check-errs

Checks Linux kernel code to see if it uses the results from the  
functions in linux/err.h. Also checks to see if code uses the results  
from functions that directly return a value from one of these error  
functions.

检查 Linux 内核代码是否使用了 linux/err.h 中函数的返回结果。  
同时也检查代码是否使用了那些直接返回这些错误函数值的函数的返回结果。

This is important in the Linux kernel because ERR_PTR, PTR_ERR,  
IS_ERR, IS_ERR_OR_NULL, ERR_CAST, and PTR_ERR_OR_ZERO return  
values must be checked, since positive pointers and negative error codes  
are being used in the same context. These functions are marked with  
**attribute**((warn_unused_result)), but some kernel versions do not  
have this warning enabled for clang.

这在 Linux 内核中非常重要，因为 ERR_PTR、PTR_ERR、IS_ERR、  
IS_ERR_OR_NULL、ERR_CAST 和 PTR_ERR_OR_ZERO 的返回值必须被检查，  
因为正指针和负错误码在同一上下文中使用。  
这些函数被标记为 **attribute**((warn_unused_result))，  
但某些内核版本在使用 clang 时未启用此警告。

Examples:  
示例：

```c
/* Trivial unused call to an ERR function */
/* 对错误函数的简单未使用调用 */
PTR_ERR_OR_ZERO(some_function_call());

/* A function that returns ERR_PTR. */
/* 一个返回 ERR_PTR 的函数 */
void *fn() { ERR_PTR(-EINVAL); }

/* An invalid use of fn. */
/* 对 fn 的无效使用 */
fn();
```
