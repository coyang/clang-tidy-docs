# cppcoreguidelines-avoid-capturing-lambda-coroutines

Flags C++20 coroutine lambdas with non-empty capture lists that may  
cause use-after-free errors and suggests avoiding captures or ensuring  
the lambda closure object has a guaranteed lifetime.

标记那些具有非空捕获列表的 C++20 协程 lambda 表达式，这些表达式可能导致使用已释放内存（use-after-free）的问题，并建议避免捕获或确保 lambda 闭包对象具有可靠的生命周期。

This check implements  
CP.51 from the C++ Core Guidelines.

此检查实现了 C++ 核心指南中的 CP.51 条款。  
[CP.51](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rcoro-capture)

Using coroutine lambdas with non-empty capture lists can be risky, as  
capturing variables can lead to accessing freed memory after the first  
suspension point. This issue can occur even with refcounted smart  
pointers and copyable types. When a lambda expression creates a  
coroutine, it results in a closure object with storage, which is often  
on the stack and will eventually go out of scope. When the closure  
object goes out of scope, its captures also go out of scope. While  
normal lambdas finish executing before this happens, coroutine lambdas  
may resume from suspension after the closure object has been destructed,  
resulting in use-after-free memory access for all captures.

使用具有非空捕获列表的协程 lambda 表达式是有风险的，因为捕获的变量可能在第一次挂起点之后被访问已释放的内存。即使使用引用计数的智能指针或可复制类型，也可能出现此问题。当 lambda 表达式创建协程时，会生成一个具有存储空间的闭包对象，该对象通常位于栈上，并最终会超出作用域。当闭包对象超出作用域时，其捕获的变量也会随之失效。普通 lambda 表达式在闭包对象销毁前就已执行完毕，而协程 lambda 表达式可能在闭包对象销毁后恢复执行，从而导致对所有捕获变量的 use-after-free 内存访问。

Consider the following example:

请参考以下示例：

```c++
int value = get_value();
std::shared_ptr<Foo> sharedFoo = get_foo();
{
    const auto lambda = [value, sharedFoo]() -> std::future<void>
    {
        co_await something();
        // "sharedFoo" and "value" have already been destroyed
        // the "shared" pointer didn't accomplish anything
    };
    lambda();
} // the lambda closure object has now gone out of scope
```

In this example, the lambda object is defined with two captures: value  
and sharedFoo. When lambda() is called, the lambda object is created  
on the stack, and the captures are copied into the closure object. When  
the coroutine is suspended, the lambda object goes out of scope, and the  
closure object is destroyed. When the coroutine is resumed, the captured  
variables may have been destroyed, resulting in use-after-free bugs.

在此示例中，lambda 对象定义了两个捕获变量：value 和 sharedFoo。当调用 lambda() 时，lambda 对象在栈上创建，并将捕获的变量复制到闭包对象中。当协程挂起时，lambda 对象超出作用域，闭包对象被销毁。当协程恢复执行时，捕获的变量可能已经被销毁，从而导致 use-after-free 错误。

In conclusion, the use of coroutine lambdas with non-empty capture lists  
can lead to use-after-free errors when resuming the coroutine after the  
closure object has been destroyed. This check helps prevent such errors  
by flagging C++20 coroutine lambdas with non-empty capture lists and  
suggesting avoiding captures or ensuring the lambda closure object has a  
guaranteed lifetime.

总之，使用具有非空捕获列表的协程 lambda 表达式，在闭包对象被销毁后恢复协程时，可能会导致 use-after-free 错误。此检查通过标记具有非空捕获列表的 C++20 协程 lambda 表达式，并建议避免捕获或确保 lambda 闭包对象具有可靠的生命周期，从而帮助防止此类错误。

Following these guidelines can help ensure the safe and reliable use of  
coroutine lambdas in C++ code.

遵循这些指南有助于在 C++ 代码中安全可靠地使用协程 lambda 表达式。
