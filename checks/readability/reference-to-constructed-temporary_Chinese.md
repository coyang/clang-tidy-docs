# readability-reference-to-constructed-temporary

# readability-reference-to-constructed-temporary（可读性 - 引用已构造的临时对象）

Detects C++ code where a reference variable is used to extend the  
lifetime of a temporary object that has just been constructed.

检测 C++ 代码中使用引用变量来延长刚刚构造的临时对象生命周期的情况。

This construction is often the result of multiple code refactorings or a  
lack of developer knowledge, leading to confusion or subtle bugs. In  
most cases, what the developer really wanted to do is create a new  
variable rather than extending the lifetime of a temporary object.

这种写法通常是多次代码重构或开发者知识不足的结果，容易导致混淆或隐蔽的错误。在大多数情况下，开发者真正想做的是创建一个新变量，而不是延长临时对象的生命周期。

Examples of problematic code include:

问题代码示例如下：

```c++
const std::string& str("hello");

struct Point { int x; int y; };
const Point& p = { 1, 2 };
```

In the first example, a const std::string& reference variable str is  
assigned a temporary object created by the std::string("hello")  
constructor. In the second example, a const Point& reference variable  
p is assigned an object that is constructed from an initializer list  
{ 1, 2 }. Both of these examples extend the lifetime of the temporary  
object to the lifetime of the reference variable, which can make it  
difficult to reason about and may lead to subtle bugs or  
misunderstanding.

在第一个示例中，const std::string& 类型的引用变量 str 被赋值为由 std::string("hello") 构造函数创建的临时对象。在第二个示例中，const Point& 类型的引用变量 p 被赋值为由初始化列表 { 1, 2 } 构造的对象。这两个示例都将临时对象的生命周期延长到了引用变量的生命周期，这可能会导致难以理解的行为，甚至引发隐蔽的错误或误解。

To avoid these issues, it is recommended to change the reference  
variable to a (const) value variable.

为避免这些问题，建议将引用变量改为（const）值变量。
