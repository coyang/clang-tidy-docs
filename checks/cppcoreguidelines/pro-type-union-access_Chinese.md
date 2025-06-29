# cppcoreguidelines-pro-type-union-access

This check flags all access to members of unions. Passing unions as a whole is not flagged.

此检查会标记对联合体成员的所有访问操作。但传递整个联合体则不会被标记。

Reading from a union member assumes that member was the last one written, and writing to a union member assumes another member with a nontrivial destructor had its destructor called. This is fragile because it cannot generally be enforced to be safe in the language and so relies on programmer discipline to get it right.

从联合体成员读取数据时，假设该成员是最后被写入的；向联合体成员写入数据时，假设另一个具有非平凡析构函数的成员的析构函数已经被调用。这种做法非常脆弱，因为在语言层面上通常无法强制保证其安全性，因此依赖程序员的自律来确保正确性。

This rule is part of the Type safety (Type.7) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的类型安全（Type.7）规范的一部分。
