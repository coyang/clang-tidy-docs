# cert-mem57-cpp

This check flags uses of default operator new where the type has extended alignment (an alignment greater than the fundamental alignment). (The default operator new is guaranteed to provide the correct alignment if the requested alignment is less or equal to the fundamental alignment). Only cases are detected (by design) where the operator new is not user-defined and is not a placement new (the reason is that in these cases we assume that the user provided the correct memory allocation).

本检查会标记在类型具有扩展对齐（即对齐方式大于基本对齐方式）时使用默认 operator new 的情况。（如果请求的对齐方式小于或等于基本对齐方式，默认的 operator new 能够保证提供正确的对齐方式。）仅检测 operator new 不是用户自定义且不是定位 new 的情况（这是设计使然，因为在这些情况下我们假设用户提供了正确的内存分配方式）。

This check corresponds to the CERT C++ Coding Standard rule MEM57-CPP. Avoid using default operator new for over-aligned types.

此检查对应于 CERT C++ 编码标准规则 MEM57-CPP：避免对超对齐类型使用默认的 operator new。

链接：https://wiki.sei.cmu.edu/confluence/display/cplusplus/MEM57-CPP.+Avoid+using+default+operator+new+for+over-aligned+types
