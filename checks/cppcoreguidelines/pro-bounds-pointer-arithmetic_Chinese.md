# cppcoreguidelines-pro-bounds-pointer-arithmetic

This check flags all usage of pointer arithmetic, because it could lead  
to an invalid pointer. Subtraction of two pointers is not flagged by  
this check.

此检查会标记所有指针运算的使用，因为这可能导致无效指针。两个指针之间的相减操作不会被此检查标记。

Pointers should only refer to single objects, and pointer arithmetic is  
fragile and easy to get wrong. span<T> is a bounds-checked, safe type  
for accessing arrays of data.

指针应仅指向单个对象，而指针运算非常脆弱且容易出错。span<T> 是一种带边界检查的安全类型，用于访问数据数组。

This rule is part of the Bounds safety (Bounds 1)  
profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的边界安全（Bounds 1）配置文件的一部分。  
[边界安全（Bounds 1）](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Pro-bounds-arithmetic)
