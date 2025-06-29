# concurrency-thread-canceltype-asynchronous

Finds pthread_setcanceltype function calls where a thread's cancellation type is set to asynchronous. Asynchronous cancellation type (PTHREAD_CANCEL_ASYNCHRONOUS) is generally unsafe, use type PTHREAD_CANCEL_DEFERRED instead which is the default. Even with deferred cancellation, a cancellation point in an asynchronous signal handler may still be acted upon and the effect is as if it was an asynchronous cancellation.

查找将线程取消类型设置为异步的 pthread_setcanceltype 函数调用。异步取消类型（PTHREAD_CANCEL_ASYNCHRONOUS）通常不安全，建议使用默认的 PTHREAD_CANCEL_DEFERRED 类型。即使使用延迟取消，在异步信号处理程序中的取消点仍可能被执行，其效果就像是异步取消一样。

```c++
pthread_setcanceltype(PTHREAD_CANCEL_ASYNCHRONOUS, &oldtype);
```

This check corresponds to the CERT C Coding Standard rule POS47-C. Do not use threads that can be canceled asynchronously.

此检查对应于 CERT C 编码标准规则 POS47-C：不要使用可以被异步取消的线程。

[cert-pos47-c]{.title-ref} redirects here as an alias of this check.

[cert-pos47-c]{.title-ref} 是此检查的别名，重定向至此。
