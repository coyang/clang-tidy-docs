# bugprone-incorrect-enable-shared-from-this

Detect classes or structs that do not publicly inherit from  
std::enable_shared_from_this, because unintended behavior will  
otherwise occur when calling shared_from_this.

检测未从 std::enable_shared_from_this 公有继承的类或结构体，因为在调用 shared_from_this 时会导致意外行为。

Consider the following code:

请参考以下代码：

````c++
#include <memory>

// private inheritance
class BadExample : std::enable_shared_from_this<BadExample> {

// ``shared_from_this``` unintended behaviour
// `libstdc++` implementation returns uninitialized ``weak_ptr``
    public:
    BadExample* foo() { return shared_from_this().get(); }
    void bar() { return; }
};

void using_not_public() {
    auto bad_example = std::make_shared<BadExample>();
    auto* b_ex = bad_example->foo();
    b_ex->bar();
}
````

Using libstdc++ implementation, shared_from_this will  
throw std::bad_weak_ptr. When using_not_public() is called, this  
code will crash without exception handling.

在使用 libstdc++ 实现时，shared_from_this 会抛出 std::bad_weak_ptr 异常。  
当调用 using_not_public() 时，如果没有异常处理，该代码将会崩溃。
