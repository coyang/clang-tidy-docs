# performance-noexcept-swap

The check flags user-defined swap and iter_swap functions not marked with noexcept or marked with noexcept(expr) where expr evaluates to false (but is not a false literal itself).

该检查会标记那些未使用 noexcept 标记，或使用了 noexcept(expr) 但 expr 的求值结果为 false（但 expr 本身不是 false 字面量）的用户自定义 swap 和 iter_swap 函数。

When a swap or iter_swap function is marked as noexcept, it assures the compiler that no exceptions will be thrown during the swapping of two objects, which allows the compiler to perform certain optimizations such as omitting exception handling code.

当 swap 或 iter_swap 函数被标记为 noexcept 时，它向编译器保证在交换两个对象的过程中不会抛出异常，这使得编译器可以执行某些优化，例如省略异常处理代码。
