# bugprone-undelegated-constructor

Finds creation of temporary objects in constructors that look like a  
function call to another constructor of the same class.

在构造函数中查找创建临时对象的情况，这些情况看起来像是调用了同一类的另一个构造函数。

The user most likely meant to use a delegating constructor or base class  
initializer.

用户很可能是想使用委托构造函数或基类初始化器。
