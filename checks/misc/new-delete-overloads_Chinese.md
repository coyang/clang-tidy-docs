# misc-new-delete-overloads

[cert-dcl54-cpp]{.title-ref} redirects here as an alias for this check.

[cert-dcl54-cpp]{.title-ref} 是此检查项的别名，重定向至此。

The check flags overloaded operator new() and operator delete() functions that do not have a corresponding free store function defined within the same scope. For instance, the check will flag a class implementation of a non-placement operator new() when the class does not also define a non-placement operator delete() function as well.

该检查会标记那些重载了 operator new() 和 operator delete() 函数，但在同一作用域中未定义对应自由存储函数的情况。例如，如果一个类实现了非定位版本的 operator new()，但未同时定义非定位版本的 operator delete()，则该检查会将其标记出来。

The check does not flag implicitly-defined operators, deleted or private operators, or placement operators.

该检查不会标记隐式定义的运算符、被删除或私有的运算符，或定位版本的运算符。

This check corresponds to CERT C++ Coding Standard rule DCL54-CPP. Overload allocation and deallocation functions as a pair in the same scope.

该检查对应于 CERT C++ 编码标准规则 DCL54-CPP：在同一作用域中成对重载内存分配和释放函数。

https://www.securecoding.cert.org/confluence/display/cplusplus/DCL54-CPP.+Overload+allocation+and+deallocation+functions+as+a+pair+in+the+same+scope

https://www.securecoding.cert.org/confluence/display/cplusplus/DCL54-CPP.+Overload+allocation+and+deallocation+functions+as+a+pair+in+the+same+scope
