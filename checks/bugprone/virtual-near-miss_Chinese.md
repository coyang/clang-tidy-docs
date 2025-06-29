# bugprone-virtual-near-miss

Warn if a function is a near miss (i.e. the name is very similar and the function signature is the same) to a virtual function from a base class.

如果一个函数与基类中的虚函数名称非常相似且函数签名相同，则发出警告（即该函数可能是一个“近似命中”，可能是开发者意图重写但拼写错误的函数）。

Example:  
示例：

```c++
struct Base {
  virtual void func();
};

struct Derived : Base {
  virtual void funk();
  // warning: 'Derived::funk' has a similar name and the same signature as virtual method 'Base::func'; did you mean to override it?
  // 警告：'Derived::funk' 的名称与虚函数 'Base::func' 非常相似，且函数签名相同；你是想重写它吗？
};
```
