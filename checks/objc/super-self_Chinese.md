# objc-super-self

Finds invocations of -self on super instances in initializers of subclasses of NSObject and recommends calling a superclass initializer instead.

在 NSObject 子类的初始化方法中查找对 super 实例调用 -self 的情况，并建议改为调用父类的初始化方法。

Invoking -self on super instances in initializers is a common programmer error when the programmer's original intent is to call a superclass initializer. Failing to call a superclass initializer breaks initializer chaining and can result in invalid object initialization.

在初始化方法中对 super 实例调用 -self 是程序员常见的错误，通常原本是想调用父类的初始化方法。如果没有调用父类的初始化方法，会破坏初始化链，可能导致对象初始化无效。
