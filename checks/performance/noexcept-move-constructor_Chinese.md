# performance-noexcept-move-constructor

The check flags user-defined move constructors and assignment operators not marked with noexcept or marked with noexcept(expr) where expr evaluates to false (but is not a false literal itself).

该检查会标记那些未使用 noexcept 标记的用户自定义移动构造函数和赋值运算符，或者使用了 noexcept(expr) 标记但 expr 的计算结果为 false（但 expr 本身不是 false 字面量）的情况。

Move constructors of all the types used with STL containers, for example, need to be declared noexcept. Otherwise STL will choose copy constructors instead. The same is valid for move assignment operations.

例如，所有与 STL 容器一起使用的类型的移动构造函数都需要声明为 noexcept。否则，STL 将选择使用拷贝构造函数。对于移动赋值操作也是如此。
