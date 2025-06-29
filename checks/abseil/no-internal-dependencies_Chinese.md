# abseil-no-internal-dependencies

Warns if code using Abseil depends on internal details. If something is in a namespace that includes the word "internal", code is not allowed to depend upon it because it's an implementation detail. They cannot friend it, include it, you mention it or refer to it in any way. Doing so violates Abseil's compatibility guidelines and may result in breakage. See <https://abseil.io/about/compatibility> for more information.

如果使用 Abseil 的代码依赖于其内部实现细节，则会发出警告。如果某个内容位于包含 "internal" 单词的命名空间中，则代码不允许依赖它，因为它属于实现细节。不能将其作为友元、包含它、引用它或以任何方式提及它。这样做违反了 Abseil 的兼容性指南，可能会导致程序出错。更多信息请参见 <https://abseil.io/about/compatibility>。

The following cases will result in warnings:

以下情况将会触发警告：

```c++
absl::strings_internal::foo();
// warning triggered on this line
// 此行触发警告

class foo {
  friend struct absl::container_internal::faa;
  // warning triggered on this line
  // 此行触发警告
};

absl::memory_internal::MakeUniqueResult();
// warning triggered on this line
// 此行触发警告
```
