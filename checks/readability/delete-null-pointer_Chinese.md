# readability-delete-null-pointer

Checks the if statements where a pointer's existence is checked and then deletes the pointer. The check is unnecessary as deleting a null pointer has no effect.

检查那些在删除指针之前先判断指针是否存在的 if 语句。这样的检查是多余的，因为删除空指针（null pointer）不会产生任何效果。

```c++
int *p;
if (p)
  delete p;
```

```c++
int *p;
if (p)
  delete p;
```
