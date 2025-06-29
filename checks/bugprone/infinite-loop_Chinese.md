# bugprone-infinite-loop

Finds obvious infinite loops (loops where the condition variable is not changed at all).

查找明显的死循环（即循环条件变量完全未被修改的循环）。

Finding infinite loops is well-known to be impossible (halting problem). However, it is possible to detect some obvious infinite loops, for example, if the loop condition is not changed. This check detects such loops. A loop is considered infinite if it does not have any loop exit statement (break, continue, goto, return, throw or a call to a function called as [[noreturn]]) and all of the following conditions hold for every variable in the condition:

众所周知，检测所有死循环是不可能的（停机问题）。然而，可以检测出一些明显的死循环，例如循环条件未被修改的情况。此检查项用于检测这类循环。如果一个循环没有任何退出语句（如 break、continue、goto、return、throw 或调用被标记为 [[noreturn]] 的函数），并且循环条件中的每个变量都满足以下所有条件，则该循环被视为死循环：

- It is a local variable.
- 它是一个局部变量。

- It has no reference or pointer aliases.
- 它没有引用或指针别名。

- It is not a structure or class member.
- 它不是结构体或类的成员变量。

Furthermore, the condition must not contain a function call to consider the loop infinite since functions may return different values for different calls.

此外，为了将循环视为死循环，循环条件中不能包含函数调用，因为函数在不同调用中可能返回不同的值。

For example, the following loop is considered infinite i is not changed in the body:

例如，以下循环被视为死循环，因为 i 在循环体中未被修改：

```c++
int i = 0, j = 0;
while (i < 10) {
  ++j;
}
```
