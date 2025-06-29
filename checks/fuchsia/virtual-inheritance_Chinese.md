# fuchsia-virtual-inheritance

Warns if classes are defined with virtual inheritance.

如果类使用虚继承进行定义，则会发出警告。

For example, classes should not be defined with virtual inheritance:

例如，类不应使用虚继承进行定义：

```c++
class B : public virtual A {};   // warning
```

See the features disallowed in Fuchsia at  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=en>

请参阅 Fuchsia 中不允许使用的特性：  
<https://fuchsia.dev/fuchsia-src/development/languages/c-cpp/cxx?hl=zh-cn>
