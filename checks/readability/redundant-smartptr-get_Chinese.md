# readability-redundant-smartptr-get

Find and remove redundant calls to smart pointer's .get() method.

查找并移除对智能指针 .get() 方法的冗余调用。

Examples:

示例：

```c++
ptr.get()->Foo()  ==>  ptr->Foo()
*ptr.get()  ==>  *ptr
*ptr->get()  ==>  **ptr
if (ptr.get() == nullptr) ... => if (ptr == nullptr) ...
```

## Options

## 选项

::: option
IgnoreMacros

If this option is set to true (default is true), the check will not warn about calls inside macros.

如果此选项设置为 true（默认值为 true），则该检查不会对宏内部的调用发出警告。
:::
