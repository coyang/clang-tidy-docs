# misc-unused-using-decls

Finds unused using declarations.

查找未使用的 using 声明。

Unused using declarations in header files will not be diagnosed since these using declarations are part of the header's public API. Allowed header file extensions can be configured via the global option HeaderFileExtensions.

头文件中的未使用 using 声明不会被诊断，因为这些 using 声明是头文件公共 API 的一部分。允许的头文件扩展名可以通过全局选项 HeaderFileExtensions 进行配置。

Example:

示例：

```c++
// main.cpp
namespace n { class C; }
using n::C;  // Never actually used.
             // 实际上从未使用过。
```
