# bugprone-multiple-new-in-one-expression

Finds multiple new operator calls in a single expression, where the allocated memory by the first new may leak if the second allocation fails and throws exception.

查找在单个表达式中多次调用 new 运算符的情况，如果第二次分配失败并抛出异常，则第一次分配的内存可能会泄漏。

C++ does often not specify the exact order of evaluation of the operands of an operator or arguments of a function. Therefore if a first allocation succeeds and a second fails, in an exception handler it is not possible to tell which allocation has failed and free the memory. Even if the order is fixed the result of a first new may be stored in a temporary location that is not reachable at the time when a second allocation fails. It is best to avoid any expression that contains more than one operator new call, if exception handling is used to check for allocation errors.

C++ 通常不指定运算符操作数或函数参数的确切求值顺序。因此，如果第一次分配成功而第二次失败，在异常处理程序中将无法判断是哪次分配失败，也无法释放内存。即使求值顺序是固定的，第一次 new 的结果也可能被存储在在第二次分配失败时无法访问的临时位置中。如果使用异常处理来检查分配错误，最好避免在一个表达式中包含多个 operator new 调用。

Different rules apply for are the short-circuit operators || and && and the , operator, where evaluation of one side must be completed before the other starts. Expressions of a list-initialization (initialization or construction using { and } characters) are evaluated in fixed order. Similarly, condition of a ? operator is evaluated before the branches are evaluated.

对于短路运算符 || 和 && 以及 , 运算符，适用不同的规则：一侧的求值必须在另一侧开始之前完成。列表初始化（使用 { 和 } 字符进行初始化或构造）的表达式按固定顺序求值。类似地，三元运算符 ? 的条件部分在分支求值之前被求值。

The check reports warning if two new calls appear in one expression at different sides of an operator, or if new calls appear in different arguments of a function call (that can be an object construction with () syntax). These new calls can be nested at any level. For any warning to be emitted the new calls should be in a code block where exception handling is used with catch for std::bad_alloc or std::exception. At ||, &&, ,, ? (condition and one branch) operators no warning is emitted. No warning is emitted if both of the memory allocations are not assigned to a variable or not passed directly to a function. The reason is that in this case the memory may be intentionally not freed or the allocated objects can be self-destructing objects.

当两个 new 调用出现在一个表达式中且位于运算符的不同侧，或出现在函数调用的不同参数中（包括使用 () 语法的对象构造）时，该检查会报告警告。这些 new 调用可以嵌套在任意层级。要触发警告，这些 new 调用必须位于使用了异常处理并捕获 std::bad_alloc 或 std::exception 的代码块中。对于 ||、&&、,、?（条件和一个分支）运算符，不会发出警告。如果两个内存分配的结果都没有被赋值给变量或直接传递给函数，也不会发出警告。原因是，在这种情况下，内存可能是故意不释放的，或者分配的对象可能是自我销毁的对象。

Examples:

示例：

```c++
struct A {
  int Var;
};
struct B {
  B();
  B(A *);
  int Var;
};
struct C {
  int *X1;
  int *X2;
};

void f(A *, B *);
int f1(A *);
int f1(B *);
bool f2(A *);

void foo() {
  A *PtrA;
  B *PtrB;
  try {
    // Allocation of 'B'/'A' may fail after memory for 'A'/'B' was allocated.
    f(new A, new B); // warning: memory allocation may leak if an other allocation is sequenced after it and throws an exception; order of these allocations is undefined

    // 分配 'A'/'B' 的内存后，'B'/'A' 的分配可能失败。
    f(new A, new B); // 警告：如果另一个分配在其之后进行并抛出异常，内存分配可能会泄漏；这些分配的顺序未定义

    // List (aggregate) initialization is used.
    C C1{new int, new int}; // no warning

    // 使用了列表（聚合）初始化。
    C C1{new int, new int}; // 无警告

    // Allocation of 'B'/'A' may fail after memory for 'A'/'B' was allocated but not yet passed to function 'f1'.
    int X = f1(new A) + f1(new B); // warning: memory allocation may leak if an other allocation is sequenced after it and throws an exception; order of these allocations is undefined

    // 分配 'A'/'B' 的内存后，'B'/'A' 的分配可能失败，但尚未传递给函数 'f1'。
    int X = f1(new A) + f1(new B); // 警告：如果另一个分配在其之后进行并抛出异常，内存分配可能会泄漏；这些分配的顺序未定义

    // Allocation of 'B' may fail after memory for 'A' was allocated.
    // From C++17 on memory for 'B' is allocated first but still may leak if allocation of 'A' fails.
    PtrB = new B(new A); // warning: memory allocation may leak if an other allocation is sequenced after it and throws an exception

    // 分配 'A' 的内存后，'B' 的分配可能失败。
    // 从 C++17 开始，'B' 的内存首先被分配，但如果 'A' 的分配失败，仍可能发生泄漏。
    PtrB = new B(new A); // 警告：如果另一个分配在其之后进行并抛出异常，内存分配可能会泄漏

    // 'new A' and 'new B' may be performed in any order.
    // 'new B'/'new A' may fail after memory for 'A'/'B' was allocated but not assigned to 'PtrA'/'PtrB'.
    (PtrA = new A)->Var = (PtrB = new B)->Var; // warning: memory allocation may leak if an other allocation is sequenced after it and throws an exception; order of these allocations is undefined

    // 'new A' 和 'new B' 的执行顺序可能是任意的。
    // 在分配了 'A'/'B' 的内存但尚未赋值给 'PtrA'/'PtrB' 之前，'new B'/'new A' 的分配可能失败。
    (PtrA = new A)->Var = (PtrB = new B)->Var; // 警告：如果另一个分配在其之后进行并抛出异常，内存分配可能会泄漏；这些分配的顺序未定义

    // Evaluation of 'f2(new A)' must be finished before 'f1(new B)' starts.
    // If 'new B' fails the allocated memory for 'A' is supposedly handled correctly because function 'f2' could take the ownership.
    bool Z = f2(new A) || f1(new B); // no warning

    // 'f2(new A)' 的求值必须在 'f1(new B)' 开始之前完成。
    // 如果 'new B' 失败，'A' 的已分配内存应已被正确处理，因为函数 'f2' 可能已获取其所有权。
    bool Z = f2(new A) || f1(new B); // 无警告

    X = (f2(new A) ? f1(new A) : f1(new B)); // no warning

    X = (f2(new A) ? f1(new A) : f1(new B)); // 无警告

    // No warning if the result of both allocations is not passed to a function
    // or stored in a variable.
    (new A)->Var = (new B)->Var; // no warning

    // 如果两个分配的结果都没有传递给函数或存储到变量中，则不会发出警告。
    (new A)->Var = (new B)->Var; // 无警告

    // No warning if at least one non-throwing allocation is used.
    f(new(std::nothrow) A, new B); // no warning

    // 如果至少有一个使用了不抛出异常的分配，则不会发出警告。
    f(new(std::nothrow) A, new B); // 无警告
  } catch(std::bad_alloc) {
  }

  // No warning if the allocation is outside a try block (or no catch handler exists for std::bad_alloc).
  // (The fact if exceptions can escape from 'foo' is not taken into account.)
  f(new A, new B); // no warning

  // 如果分配发生在 try 块之外（或没有 std::bad_alloc 的 catch 处理程序），则不会发出警告。
  //（不会考虑异常是否可能从 'foo' 中逃逸的情况。）
  f(new A, new B); // 无警告
}
```
