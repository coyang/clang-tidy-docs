# bugprone-suspicious-realloc-usage

This check finds usages of realloc where the return value is assigned to the same expression as passed to the first argument: p = realloc(p, size); The problem with this construct is that if realloc fails it returns a null pointer but does not deallocate the original memory. If no other variable is pointing to it, the original memory block is not available any more for the program to use or free. In either case p = realloc(p, size); indicates bad coding style and can be replaced by q = realloc(p, size);.

该检查会发现将 realloc 的返回值赋值给与其第一个参数相同的表达式的用法：p = realloc(p, size); 这种写法的问题在于，如果 realloc 失败，它会返回一个空指针，但不会释放原有的内存。如果没有其他变量指向原内存块，那么程序将无法再使用或释放这块内存。无论哪种情况，p = realloc(p, size); 都表示一种不良的编码风格，应改为 q = realloc(p, size);。

The pointer expression (used at realloc) can be a variable or a field member of a data structure, but can not contain function calls or unresolved types.

realloc 中使用的指针表达式可以是变量或数据结构的字段成员，但不能包含函数调用或未解析的类型。

In obvious cases when the pointer used at realloc is assigned to another variable before the realloc call, no warning is emitted. This happens only if a simple expression in form of q = p or void \*q = p is found in the same function where p = realloc(p, ...) is found. The assignment has to be before the call to realloc (but otherwise at any place) in the same function. This suppression works only if p is a single variable.

在明显的情况下，如果在调用 realloc 之前将用于 realloc 的指针赋值给另一个变量，则不会发出警告。只有当在与 p = realloc(p, ...) 相同的函数中，存在类似 q = p 或 void \*q = p 的简单赋值表达式时，才会抑制警告。该赋值必须出现在 realloc 调用之前（但可以在函数中的任意位置）。这种抑制机制仅在 p 是单个变量的情况下有效。

Examples:

示例：

```c++
struct A {
  void *p;
};

A &getA();

void foo(void *p, A *a, int new_size) {
  p = realloc(p, new_size); // warning: 'p' may be set to null if 'realloc' fails, which may result in a leak of the original buffer
                            // 警告：如果 realloc 失败，'p' 可能被设置为 null，这可能导致原始缓冲区泄漏
  a->p = realloc(a->p, new_size); // warning: 'a->p' may be set to null if 'realloc' fails, which may result in a leak of the original buffer
                                 // 警告：如果 realloc 失败，'a->p' 可能被设置为 null，这可能导致原始缓冲区泄漏
  getA().p = realloc(getA().p, new_size); // no warning
                                         // 无警告
}

void foo1(void *p, int new_size) {
  void *p1 = p;
  p = realloc(p, new_size); // no warning
                            // 无警告
}
```
