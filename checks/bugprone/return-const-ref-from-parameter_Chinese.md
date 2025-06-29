# bugprone-return-const-ref-from-parameter

Detects return statements that return a constant reference parameter as constant reference. This may cause use-after-free errors if the caller uses xvalues as arguments.

检测将常量引用参数作为常量引用返回的 return 语句。如果调用者使用 xvalue 作为参数，这可能会导致使用已释放内存（use-after-free）错误。

In C++, constant reference parameters can accept xvalues which will be destructed after the call. When the function returns such a parameter also as constant reference, then the returned reference can be used after the object it refers to has been destroyed.

在 C++ 中，常量引用参数可以接受将在函数调用后被销毁的 xvalue。当函数将这样的参数作为常量引用返回时，返回的引用可能在其所引用的对象被销毁后仍被使用。

## Example

示例

```c++
struct S {
  int v;
  S(int);
  ~S();
};

const S &fn(const S &a) {
  return a;
}

const S& s = fn(S{1});
s.v; // use after free
```

This issue can be resolved by declaring an overload of the problematic function where the const & parameter is instead declared as &&. The developer has to ensure that the implementation of that function does not produce a use-after-free, the exact error that this check is warning against. Marking such an && overload as deleted, will silence the warning as well. In the case of different const & parameters being returned depending on the control flow of the function, an overload where all problematic const & parameters have been declared as && will resolve the issue.

可以通过为存在问题的函数声明一个重载版本来解决此问题，在该重载中将 const & 参数改为 &&。开发者需要确保该函数的实现不会导致 use-after-free 错误，也就是本检查所警告的问题。将这样的 && 重载标记为 deleted 也可以消除警告。如果函数根据控制流返回不同的 const & 参数，那么为所有存在问题的 const & 参数声明为 && 的重载也可以解决此问题。

This issue can also be resolved by adding [[clang::lifetimebound]]. Clang enable -Wdangling warning by default which can detect mis-uses of the annotated function. See lifetimebound attribute for details.

此问题也可以通过添加 [[clang::lifetimebound]] 属性来解决。Clang 默认启用 -Wdangling 警告，可以检测对带有该注解函数的误用。详情请参阅 lifetimebound 属性。

```c++
const int &f(const int &a [[clang::lifetimebound]]) { return a; } // no warning
const int &v = f(1); // warning: temporary bound to local reference 'v' will be destroyed at the end of the full-expression [-Wdangling]
```
