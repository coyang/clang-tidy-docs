# performance-no-automatic-move

Finds local variables that cannot be automatically moved due to constness.

查找由于 const 限定导致无法自动移动的局部变量。

Under certain conditions, local values are automatically moved out when returning from a function. A common mistake is to declare local lvalue variables const, which prevents the move.

在某些条件下，函数返回时局部变量的值会被自动移动出去。一个常见的错误是将局部左值变量声明为 const，这会阻止自动移动的发生。

Example [1]:

示例 [1]：

```c++
StatusOr<std::vector<int>> Cool() {
  std::vector<int> obj = ...;
  return obj;  // calls StatusOr::StatusOr(std::vector<int>&&)
}

StatusOr<std::vector<int>> NotCool() {
  const std::vector<int> obj = ...;
  return obj;  // calls `StatusOr::StatusOr(const std::vector<int>&)`
}
```

The former version (Cool) should be preferred over the latter (NotCool) as it will avoid allocations and potentially large memory copies.

应优先使用前者（Cool）而不是后者（NotCool），因为它可以避免内存分配和可能的大量内存拷贝。

## Semantics

语义

In the example above, StatusOr::StatusOr(T&&) have the same semantics as long as the copy and move constructors for T have the same semantics. Note that there is no guarantee that S::S(T&&) and S::S(const T&) have the same semantics for any single S, so we're not providing automated fixes for this check, and judgement should be exerted when making the suggested changes.

在上述示例中，只要类型 T 的拷贝构造函数和移动构造函数具有相同的语义，StatusOr::StatusOr(T&&) 就具有相同的语义。请注意，对于任意类型 S，并不能保证 S::S(T&&) 和 S::S(const T&) 具有相同的语义，因此我们不会为此检查提供自动修复，进行建议修改时应谨慎判断。

## -Wreturn-std-move

-Wreturn-std-move

Another case where the move cannot happen is the following:

另一个无法发生移动的情况如下：

```c++
StatusOr<std::vector<int>> Uncool() {
  std::vector<int>&& obj = ...;
  return obj;  // calls `StatusOr::StatusOr(const std::vector<int>&)`
}
```

In that case the fix is more consensual: just return std::move(obj). This is handled by the -Wreturn-std-move warning.

在这种情况下，修复方法更为一致：只需 return std::move(obj)。这由 -Wreturn-std-move 警告处理。
