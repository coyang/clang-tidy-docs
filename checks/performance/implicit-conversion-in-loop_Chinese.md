# performance-implicit-conversion-in-loop

This warning appears in a range-based loop with a loop variable of const  
ref type where the type of the variable does not match the one returned  
by the iterator. This means that an implicit conversion happens, which  
can for example result in expensive deep copies.

此警告出现在基于范围的循环中，当循环变量是 const 引用类型，且其类型与迭代器返回的类型不匹配时。  
这意味着发生了隐式转换，例如可能导致代价高昂的深拷贝。

Example:  
示例：

```c++
map<int, vector<string>> my_map;
for (const pair<int, vector<string>>& p : my_map) {}
// The iterator type is in fact pair<const int, vector<string>>, which means
// that the compiler added a conversion, resulting in a copy of the vectors.

// 实际上，迭代器的类型是 pair<const int, vector<string>>，
// 这意味着编译器添加了一个转换，导致 vector 被复制。
```

The easiest solution is usually to use const auto& instead of writing  
the type manually.

最简单的解决方案通常是使用 const auto&，而不是手动编写类型。
