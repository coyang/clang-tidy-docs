# google-objc-avoid-nsobject-new

Finds calls to +new or overrides of it, which are prohibited by the Google Objective-C style guide.

查找对 +new 的调用或对其的重写，这在 Google Objective-C 风格指南中是被禁止的。

The Google Objective-C style guide forbids calling +new or overriding it in class implementations, preferring +alloc and -init methods to instantiate objects.

Google Objective-C 风格指南禁止在类实现中调用或重写 +new，推荐使用 +alloc 和 -init 方法来实例化对象。

An example:

一个示例：

```objc
NSDate *now = [NSDate new];
Foo *bar = [Foo new];
```

Instead, code should use +alloc/-init or class factory methods.

相反，代码应使用 +alloc/-init 或类工厂方法。

```objc
NSDate *now = [NSDate date];
Foo *bar = [[Foo alloc] init];
```

This check corresponds to the Google Objective-C Style Guide rule Do Not Use +new.

此检查对应于 Google Objective-C 风格指南中的规则：不要使用 +new。  
链接：https://google.github.io/styleguide/objcguide.html#do-not-use-new
