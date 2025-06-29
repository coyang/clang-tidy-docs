# cppcoreguidelines-interfaces-global-init

This check flags initializers of globals that access extern objects, and  
therefore can lead to order-of-initialization problems.

此检查会标记那些在初始化全局变量时访问外部对象的情况，  
因为这可能导致初始化顺序的问题。

This check implements  
[I.22](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Ri-global-init)  
from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的  
[I.22](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Ri-global-init) 条款。

Note that currently this does not flag calls to non-constexpr functions,  
and therefore globals could still be accessed from functions themselves.

请注意，目前此检查不会标记对非 constexpr 函数的调用，  
因此全局变量仍可能在函数中被访问。
