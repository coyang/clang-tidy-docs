# objc-dealloc-in-category

Finds implementations of -dealloc in Objective-C categories. The category implementation will override any -dealloc in the class implementation, potentially causing issues.

查找 Objective-C 分类中实现的 -dealloc 方法。分类中的实现会覆盖类中已有的 -dealloc 实现，可能会导致问题。

Classes implement -dealloc to perform important actions to deallocate an object. If a category on the class implements -dealloc, it will override the class's implementation and unexpected deallocation behavior may occur.

类通过实现 -dealloc 方法来执行对象释放时的重要操作。如果类的某个分类实现了 -dealloc 方法，它将覆盖类本身的实现，可能会导致对象释放行为异常。
