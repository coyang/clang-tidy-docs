# performance-move-constructor-init

# performance-move-constructor-init

"cert-oop11-cpp" redirects here as an alias for this check.

“cert-oop11-cpp” 是此检查项的别名，会重定向到此处。

The check flags user-defined move constructors that have a ctor-initializer initializing a member or base class through a copy constructor instead of a move constructor.

该检查会标记那些在构造函数初始化列表中，通过拷贝构造函数而非移动构造函数来初始化成员或基类的用户自定义移动构造函数。
