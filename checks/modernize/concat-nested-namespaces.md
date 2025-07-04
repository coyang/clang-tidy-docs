# modernize-concat-nested-namespaces

Checks for use of nested namespaces such as
`namespace a { namespace b { ... } }` and suggests changing to the more
concise syntax introduced in C++17: `namespace a::b { ... }`. Inline
namespaces are not modified.

For example:

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
