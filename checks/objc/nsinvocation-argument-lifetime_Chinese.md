# objc-nsinvocation-argument-lifetime

Finds calls to NSInvocation methods under ARC that don't have proper argument object lifetimes. When passing Objective-C objects as parameters to the NSInvocation methods getArgument:atIndex: and getReturnValue:, the values are copied by value into the argument pointer, which leads to incorrect releasing behavior if the object pointers are not declared \_\_unsafe_unretained.

在 ARC（自动引用计数）环境下，检查对 NSInvocation 方法的调用是否具有正确的参数对象生命周期。当将 Objective-C 对象作为参数传递给 NSInvocation 的 getArgument:atIndex: 和 getReturnValue: 方法时，这些值会按值复制到参数指针中。如果对象指针未声明为 \_\_unsafe_unretained，则会导致错误的释放行为。

For code:

对于以下代码：

```objc
id arg;
[invocation getArgument:&arg atIndex:2];

__strong id returnValue;
[invocation getReturnValue:&returnValue];
```

The fix will be:

修复方式如下：

```objc
__unsafe_unretained id arg;
[invocation getArgument:&arg atIndex:2];

__unsafe_unretained id returnValue;
[invocation getReturnValue:&returnValue];
```

The check will warn on being passed instance variable references that have lifetimes other than \_\_unsafe_unretained, but does not propose a fix:

该检查器会对传递生命周期不是 \_\_unsafe_unretained 的实例变量引用发出警告，但不会提供修复建议：

```objc
// "id _returnValue" is declaration of instance variable of class.
[invocation getReturnValue:&self->_returnValue];
```
