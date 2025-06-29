# bugprone-bad-signal-to-kill-thread

Finds pthread_kill function calls when a thread is terminated by raising SIGTERM signal and the signal kills the entire process, not just the individual thread. Use any signal except SIGTERM.

查找使用 pthread_kill 函数调用并通过发送 SIGTERM 信号终止线程的情况，此信号会终止整个进程，而不仅仅是单个线程。应使用除 SIGTERM 以外的其他信号。

```c++
pthread_kill(thread, SIGTERM);
```

This check corresponds to the CERT C Coding Standard rule POS44-C. Do not use signals to terminate threads.

此检查对应于 CERT C 编码标准规则 POS44-C：不要使用信号来终止线程。

[cert-pos44-c]{.title-ref} redirects here as an alias of this check.

[cert-pos44-c]{.title-ref} 是此检查项的别名并重定向至此。
