# misc-unused-alias-decls

Finds unused namespace alias declarations.

查找未使用的命名空间别名声明。

```c++
namespace my_namespace {
class C {};
}
namespace unused_alias = ::my_namespace;
```
