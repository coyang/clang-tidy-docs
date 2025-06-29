# performance-noexcept-destructor

The check flags user-defined destructors marked with noexcept(expr)  
该检查会标记那些使用 noexcept(expr) 标记的用户自定义析构函数

where expr evaluates to false (but is not a false literal itself).  
其中 expr 的计算结果为 false（但本身并不是字面量 false）。

When a destructor is marked as noexcept, it assures the compiler that  
当析构函数被标记为 noexcept 时，它向编译器保证

no exceptions will be thrown during the destruction of an object, which  
在对象销毁过程中不会抛出异常，这使得编译器可以执行某些优化，例如

allows the compiler to perform certain optimizations such as omitting  
省略异常处理代码。

exception handling code.  
异常处理代码。
