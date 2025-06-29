# cppcoreguidelines-pro-bounds-array-to-pointer-decay

This check flags all array to pointer decays.

此检查会标记所有数组到指针的退化操作。

Pointers should not be used as arrays. span<T> is a bounds-checked, safe alternative to using pointers to access arrays.

不应将指针用作数组。span<T> 是一种带边界检查的安全替代方案，可用于访问数组而不是使用指针。

This rule is part of the Bounds safety (Bounds 3) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的边界安全（Bounds 3）配置文件的一部分。
