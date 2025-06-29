# modernize-concat-nested-namespaces

Checks for use of nested namespaces such as  
检查是否使用了嵌套命名空间，例如

namespace a { namespace b { ... } } and suggests changing to the more  
namespace a { namespace b { ... } }，并建议更改为更

concise syntax introduced in C++17: namespace a::b { ... }. Inline  
简洁的 C++17 新语法：namespace a::b { ... }。内联

namespaces are not modified.  
命名空间不会被修改。

For example:  
例如：

```c++
namespace n1 {
namespace n2 {
void t();
}
}

namespace n3 {
namespace n4 {
namespace n5 {
void t();
}
}
namespace n6 {
namespace n7 {
void t();
}
}
}

// in c++20
namespace n8 {
inline namespace n9 {
void t();
}
}
```

Will be modified to:  
将被修改为：

```c++
namespace n1::n2 {
void t();
}

namespace n3 {
namespace n4::n5 {
void t();
}
namespace n6::n7 {
void t();
}
}

// in c++20
namespace n8::inline n9 {
void t();
}
```
