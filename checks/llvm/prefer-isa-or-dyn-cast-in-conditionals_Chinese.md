# llvm-prefer-isa-or-dyn-cast-in-conditionals

Looks at conditionals and finds and replaces cases of cast<>, which will assert rather than return a null pointer, and dyn_cast<> where the return value is not captured. Additionally, finds and replaces cases that match the pattern var && isa<X>(var), where var is evaluated twice.

检查条件语句，并查找和替换 cast<> 的用法（该函数在失败时会触发断言而不是返回空指针），以及未捕获返回值的 dyn_cast<> 用法。此外，还会查找并替换匹配 var && isa<X>(var) 模式的情况，其中 var 被计算了两次。

```c++
// Finds these:
// 查找以下情况：

if (auto x = cast<X>(y)) {}
// is replaced by:
// 替换为：
if (auto x = dyn_cast<X>(y)) {}

if (cast<X>(y)) {}
// is replaced by:
// 替换为：
if (isa<X>(y)) {}

if (dyn_cast<X>(y)) {}
// is replaced by:
// 替换为：
if (isa<X>(y)) {}

if (var && isa<T>(var)) {}
// is replaced by:
// 替换为：
if (isa_and_nonnull<T>(var.foo())) {}

// Other cases are ignored, e.g.:
// 其他情况将被忽略，例如：

if (auto f = cast<Z>(y)->foo()) {}
if (cast<Z>(y)->foo()) {}
if (X.cast(y)) {}
```
