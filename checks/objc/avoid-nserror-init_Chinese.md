# objc-avoid-nserror-init

Finds improper initialization of NSError objects.

查找不正确的 NSError 对象初始化方式。

According to Apple developer document, we should always use factory method errorWithDomain:code:userInfo: to create new NSError objects instead of [NSError alloc] init]. Otherwise it will lead to a warning message during runtime.

根据 Apple 开发者文档，我们应始终使用工厂方法 errorWithDomain:code:userInfo: 来创建新的 NSError 对象，而不是使用 [NSError alloc] init]。否则在运行时会导致警告信息。

The corresponding information about NSError creation:  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ErrorHandlingCocoa/CreateCustomizeNSError/CreateCustomizeNSError.html>

有关 NSError 创建的相关信息：  
<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ErrorHandlingCocoa/CreateCustomizeNSError/CreateCustomizeNSError.html>
