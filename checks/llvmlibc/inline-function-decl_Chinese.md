# llvmlibc-inline-function-decl

Checks that all implicitly and explicitly inline functions in header files are tagged with the LIBC_INLINE macro, except for functions implicit to classes or deleted functions. See the libc style guide for more information about this macro.

检查头文件中所有隐式和显式的内联函数是否都使用了 LIBC_INLINE 宏进行标记，类中的隐式函数或被删除的函数除外。有关该宏的更多信息，请参阅 libc 风格指南。
