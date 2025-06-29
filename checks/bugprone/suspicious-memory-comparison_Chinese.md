# bugprone-suspicious-memory-comparison

Finds potentially incorrect calls to memcmp() based on properties of the arguments. The following cases are covered:

根据参数的属性，查找对 memcmp() 的潜在错误调用。涵盖以下几种情况：

Case 1: Non-standard-layout type

比较非标准布局对象的对象表示可能无法正确比较其值表示。

Case 2: Types with no unique object representation

Objects with the same value may not have the same object representation. This may be caused by padding or floating-point types.

值相同的对象可能没有相同的对象表示。这可能是由于填充字节或浮点类型造成的。

See also: EXP42-C. Do not compare padding data and FLP37-C. Do not use object representations to compare floating-point values

另请参阅：EXP42-C. 不要比较填充数据 和 FLP37-C. 不要使用对象表示来比较浮点值

This check is also related to and partially overlaps the CERT C++ Coding Standard rules OOP57-CPP. Prefer special member functions and overloaded operators to C Standard Library functions and EXP62-CPP. Do not access the bits of an object representation that are not part of the object's value representation

此检查还与 CERT C++ 编码标准中的以下规则相关，并有部分重叠：OOP57-CPP. 优先使用特殊成员函数和重载运算符而非 C 标准库函数，以及 EXP62-CPP. 不要访问对象表示中不属于对象值表示的位

cert-exp42-c redirects here as an alias of this check.

cert-exp42-c 是此检查的别名，重定向至此。
