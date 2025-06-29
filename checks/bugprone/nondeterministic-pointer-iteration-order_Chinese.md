# bugprone-nondeterministic-pointer-iteration-order

Finds nondeterministic usages of pointers in unordered containers.

查找在无序容器中使用指针的非确定性用法。

One canonical example is iteration across a container of pointers.

一个典型的例子是在指针容器上进行迭代。

```c++
{
  int a = 1, b = 2;
  std::unordered_set<int *> UnorderedPtrSet = {&a, &b};
  for (auto i : UnorderedPtrSet)
    f(i);
}
```

Another such example is sorting a container of pointers.

另一个例子是对指针容器进行排序。

```c++
{
  int a = 1, b = 2;
  std::vector<int *> VectorOfPtr = {&a, &b};
  std::sort(VectorOfPtr.begin(), VectorOfPtr.end());
}
```

Iteration of a containers of pointers may present the order of different
pointers differently across different runs of a program. In some cases
this may be acceptable behavior, in others this may be unexpected
behavior. This check is advisory for this reason.

在程序的不同运行中，指针容器的迭代可能会以不同的顺序呈现不同的指针。在某些情况下，这种行为是可以接受的，而在其他情况下，这可能是意料之外的行为。因此，此检查项是建议性的。

This check only detects range-based for loops over unordered sets and
maps. It also detects calls sorting-like algorithms on containers
holding pointers. Other similar usages will not be found and are false
negatives.

此检查仅检测对无序集合（unordered set）和映射（map）使用基于范围的 for 循环的情况。它还会检测对包含指针的容器调用类似排序算法的情况。其他类似用法不会被发现，属于漏报。

Limitations:

限制：

- This check currently does not check if a nondeterministic iteration
  order is likely to be a mistake, and instead marks all such
  iterations as bugprone.

  此检查当前不会判断非确定性迭代顺序是否可能是错误，而是将所有此类迭代标记为易出错。

- std::reference_wrapper is not considered yet.

  尚未考虑 std::reference_wrapper。

- Only for loops are considered, other iterators can be included in
  improvements.

  当前仅考虑 for 循环，其他迭代器可在后续改进中加入。
