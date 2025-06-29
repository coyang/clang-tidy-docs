# cppcoreguidelines-pro-type-member-init

The check flags user-provided constructor definitions that do not initialize all fields that would be left in an undefined state by default construction, e.g. builtins, pointers and record types without user-provided default constructors containing at least one such type. If these fields aren't initialized, the constructor will leave some of the memory in an undefined state.

该检查会标记那些用户自定义构造函数中未初始化所有字段的情况，这些字段在默认构造时会处于未定义状态，例如内建类型、指针以及不包含用户提供的默认构造函数的记录类型（其中至少包含一种此类类型）。如果这些字段未被初始化，构造函数将导致部分内存处于未定义状态。

For C++11 it suggests fixes to add in-class field initializers. For older versions it inserts the field initializers into the constructor initializer list. It will also initialize any direct base classes that need to be zeroed in the constructor initializer list.

对于 C++11，该检查建议通过类内字段初始化器进行修复。对于更早的版本，它会将字段初始化器插入构造函数的初始化列表中。它还会初始化构造函数初始化列表中需要清零的直接基类。

The check takes assignment of fields in the constructor body into account but generates false positives for fields initialized in methods invoked in the constructor body.

该检查会考虑构造函数体内对字段的赋值，但对于在构造函数体中调用的方法中初始化的字段，可能会产生误报。

The check also flags variables with automatic storage duration that have record types without a user-provided constructor and are not initialized. The suggested fix is to zero initialize the variable via {} for C++11 and beyond or = {} for older language versions.

该检查还会标记那些具有自动存储期、类型为不带用户提供构造函数的记录类型且未被初始化的变量。建议的修复方式是：对于 C++11 及以上版本使用 {} 进行零初始化，对于旧版本使用 = {}。

## Options

## 选项

::: option
IgnoreArrays

If set to true, the check will not warn about array members that are not zero-initialized during construction. For performance critical code, it may be important to not initialize fixed-size array members. Default is false.

如果设置为 true，该检查将不会对在构造过程中未进行零初始化的数组成员发出警告。对于性能敏感的代码，可能需要避免初始化固定大小的数组成员。默认值为 false。
:::

::: option
UseAssignment

If set to true, the check will provide fix-its with literal initializers ( int i = 0; ) instead of curly braces ( int i{}; ). Default is false.

如果设置为 true，该检查将提供使用字面值初始化器（如 int i = 0;）的修复建议，而不是使用花括号（如 int i{};）。默认值为 false。
:::

This rule is part of the Type safety (Type.6) profile from the C++ Core Guidelines.

此规则属于 C++ 核心指南中的类型安全（Type.6）配置文件的一部分。
