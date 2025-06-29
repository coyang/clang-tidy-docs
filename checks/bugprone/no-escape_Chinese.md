# bugprone-no-escape

Finds pointers with the noescape attribute that are captured by an asynchronously-executed block. The block arguments in dispatch_async() and dispatch_after() are guaranteed to escape, so it is an error if a pointer with the noescape attribute is captured by one of these blocks.

查找带有 noescape 属性的指针被异步执行的代码块捕获的情况。在 dispatch_async() 和 dispatch_after() 中传入的代码块参数保证会逃逸，因此如果这些代码块捕获了带有 noescape 属性的指针，则是一个错误。

The following is an example of an invalid use of the noescape attribute.

以下是一个错误使用 noescape 属性的示例。

> ```objc
> void foo(__attribute__((noescape)) int *p) {
>   dispatch_async(queue, ^{
>     *p = 123;
>   });
> });
> ```

> ```objc
> void foo(__attribute__((noescape)) int *p) {
>   dispatch_async(queue, ^{
>     *p = 123;
>   });
> });
> ```
